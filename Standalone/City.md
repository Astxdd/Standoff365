City

Информационный портал УК City
Хотите узнать больше о деятельности управляющей компании City в Государстве F? Тогда посетите ее информационный портал! Ежедневные новости, справочная информация и карта бизнес-центра - все самое важное для клиентов УК City в удобном интерфейсе.

Заходим на 10.124.249.8

В сурсе видим bludit, version 3.9.2
<img width="1108" height="604" alt="image" src="https://github.com/user-attachments/assets/302adec8-3439-4bf1-b950-9d953761a72f" />

При фаззинге ffuf -u "http://10.124.249.8/FUZZ" -w /usr/share/seclists/Discovery/Web-Content/common.txt -fs 199
находим /admin 301
<img width="794" height="572" alt="image" src="https://github.com/user-attachments/assets/373a2a1a-2493-4c8b-a71a-b8569a3ed8c3" />

В /admin видим bludit
<img width="1375" height="685" alt="image" src="https://github.com/user-attachments/assets/f9af297d-4207-4ed9-b7c6-6db10e81a625" />

Гуглим всевозможные эксплоиты и цвешки связанные с этой cms-кой и его версией
<img width="1375" height="685" alt="image" src="https://github.com/user-attachments/assets/865fcb5f-8646-41c0-876c-214cbe8e0a2b" />

Джемини показывает, что есть 2 цвешки одна для рце и одна для брутфорса. Получается что, нам скорее всего надо будет сбрутить пароль админа и с этими кредами выполнить рце на данной машине.

Нашел PoC брутфорса, использовал его и нашел пароль 696969
https://github.com/srikanthprathi/modified-exploits/blob/master/bludit-CMD-3.9.2-Brute-Force-Protection-Bypass-script.py


<img width="1375" height="685" alt="image" src="https://github.com/user-attachments/assets/b700c782-c1b4-4c39-93c3-8adb7db2b562" />

<img width="1375" height="685" alt="image" src="https://github.com/user-attachments/assets/e57a6b20-77b6-49c9-8ab1-28324cf7811c" />

Теперь нужно будет с этими кредами admin:696969 взять рце, для этого я буду использовать metasploit. В msfconsole ищем CVE-2019-16113, подготавливаем всё и запускаем и вуаля у нас рцешка
<img width="827" height="571" alt="image" src="https://github.com/user-attachments/assets/9b45215d-41d6-4c32-94ed-0c4d258d1b1a" />

Для начало нам нужно взять рута, чтобы выполнить LPE и забрать флаг. Благодаря команде getcap -r / 2>/dev/null нашли капу с python и взял рута
<img width="685" height="97" alt="image" src="https://github.com/user-attachments/assets/c56aa629-e2b8-4620-a4f1-4d973a76580e" />

Теперь получает флаг за дефейс. Переходим в /root/flags/ue и прописываем cat ue.flag и получаем наш флаг
<img width="626" height="74" alt="image" src="https://github.com/user-attachments/assets/5d205677-ce23-4a9b-9f5e-21a3258e9de0" />

На этом тачка всё, все флаги были обнаружены

<img width="1493" height="948" alt="image" src="https://github.com/user-attachments/assets/a78275ac-69d2-45ce-9762-6f8f2d45e52c" />
<img width="1435" height="725" alt="image" src="https://github.com/user-attachments/assets/2a2a0bd9-bf7b-45c3-88c9-c9fd2a3a95cc" />
