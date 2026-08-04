RND

Портал исследований металлургического комбината «МеталлиКО»
Металлургический комбинат «МеталлиКО» приглашает коллег со всего мира присоединиться к разработке передовых решений. К услугам приглашенных специалистов высокотехнологические лаборатории, грантовая поддержка и даже специально разработанный исследовательский портал.
Хост: rnd.standalone.stf 10.124.249.5
В нмапе особо ничего нет
<img width="976" height="363" alt="image" src="https://github.com/user-attachments/assets/35426e1b-86c1-4bdd-bc93-f278690282f1" />

В вебчике регаюсь и вижу, что есть /researches, которая мне недоступно. Чуть-чуть поискав варианты нахожу куки и какой-то токен
<img width="970" height="128" alt="image" src="https://github.com/user-attachments/assets/d0fd2eb1-ade6-4bac-b096-6c6b47c44f32" />

eyJfZnJlc2giOmZhbHNlLCJfdXNlcl9pZCI6MSwiY3NyZl90b2tlbiI6IjFiM2VmY2QzMjc4ZTI4Y2EzZTRkMjEzZjNmMTlmZjJlOTAxN2ZmNTkifQ.anIWzQ.t7hNjtKr0NPtDTd0tTC8IsQ8MBI

декод показал примерно вот это:
{
    "_fresh": false,
    "_user_id": ....,
    "csrf_token": "..."
}

очень похоже на Flask, поэтому используем flask-unsign

<img width="1920" height="434" alt="image" src="https://github.com/user-attachments/assets/ecb38ce1-aa20-4aab-b541-7004aaab6375" />

Находим ключ, меняем айди на первый, тоесть админ, заходим в /research и ищем флаг...

<img width="1920" height="1011" alt="image" src="https://github.com/user-attachments/assets/68d6307d-0688-43c4-8b33-b42b9aded467" />

Вот такая вот изи тачка














<img width="1446" height="699" alt="image" src="https://github.com/user-attachments/assets/46676c80-fb03-4254-bdb9-e8fbdae16879" />
