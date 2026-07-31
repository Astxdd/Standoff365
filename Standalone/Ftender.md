Ftender

Портал для управления закупками и поставщиками
Популярность компании Pipeline начала экспоненциально расти, и ручное управление
закупками перестало отвечать требованиям эффективности. Именно поэтому команда
разработки запустила новый портал, позволяющий систематизировать общение с
поставщиками товаров и услуг, а также упростить бухгалтерию.
Скоуп
ftender.standalone.stf 10.124.249.18


Первый контакт с хостом
curl -sI http://10.124.249.18/
curl -sI -H 'Host: ftender.standalone.stf' http://10.124.249.18/


nginx/1.18.0
X-Powered-By: Next.js
заголовки RSC / Next-Router → Next.js App Router


<img width="1054" height="644" alt="image" src="https://github.com/user-attachments/assets/9aa23b37-932d-445f-b34d-06b8fe64c934" />


Как устроена регистрация?
Форма register клиентский React. Поля username / password, но submit идёт не в классический <form action>, а через tRPC mutation.

Скачали чанк:

/_next/static/chunks/app/auth/register/page-....js
Внутри нашли:

p = d.F.profile.signUp.useMutation()
await p.mutateAsync({ username: e, password: s })
// success → router.push("/auth/signin")
И конфиг клиента:

url: window.location.origin + "/api/trpc"
headers: { "x-trpc-source": "nextjs-react" }
Вывод: весь бэкенд — /api/trpc/<router>.<procedure>.
Дальше стратегия: парсить бандлы на F.*.useMutation/useQuery, а не угадывать REST.

Регистрация руками:

POST /api/trpc/profile.signUp
Content-Type: application/json
{"json":{"username":"astxdd","password":"astxdd18"}}
Ответ:

{"result":{"data":{"json":{"rowsAffected":1,"lastInsertRowid":"3",...}}}
→ SQLite под капотом.

Логин NextAuth:

POST /api/auth/callback/credentials
csrfToken=...&username=astxdd&password=astxdd18
→ Set-Cookie: authjs.session-token=... (HttpOnly)
Проверка:

GET /api/auth/session
→ {"user":{"id":"0f7d7776-...","username":"astxdd"},...}
Cookie HttpOnly → если будет XSS, document.cookie не сработает. Сразу в уме: эксфиль через fetch(..., credentials) + отправка тела ответа на свой сервер.

Главный вывод разведки:

Приложение не использует стандартный REST API. Весь бэкенд общается через единую точку входа /api/trpc/<router>.<procedure>.

Вместо того чтобы фазить (брутить) скрытые REST-эндпоинты через утилиты вроде ffuf или gobuster, нужно просто выкачать все JS-бандлы и вытащить из них названия всех роутеров и процедур (F.*.useQuery / useMutation). Это даёт 100% карту бэкенд API.




