Board

Электронное табло Standalone
"AirBoard" – это современный сервис для управления информационными табло в аэропортах. Благодаря надежности, скорости работы и гибкости в настройке "AirBoard" становится незаменимым инструментом для аэропортов, стремящихся к современному уровню автоматизации и комфорта.
Пассажиры аэропорта уверены - с этим сервисом они всегда видят актуальные данные о прилетах, вылетах, задержках и изменениях в расписании.
Хост: board.standalone.stf, 10.124.249.16
Нмапим айпишник и видим два веба
<img width="777" height="355" alt="image" src="https://github.com/user-attachments/assets/11b84597-2b90-4b83-a615-abea3044064b" />

В 80 порту у нас ничего интересного нет, а в 8080 порту у нас Roundcube 1.6.9 версии. При фаззинге ffuf -u "http://10.124.249.16:8080/FUZZ" -w /usr/share/wordlists/seclists/Discovery/Web-Content/common.txt -fs 199
удалось обнаружить /logs


<img width="777" height="355" alt="image" src="https://github.com/user-attachments/assets/a0553b06-61ac-47ab-8e52-2f27f95e52c6" />




<img width="712" height="458" alt="image" src="https://github.com/user-attachments/assets/572af036-0d78-46cb-b4fa-214f2399d8c4" />




Зайдя в /logs/access.log начал перебирать почты сотрудников
<img width="1806" height="862" alt="image" src="https://github.com/user-attachments/assets/588d4d7b-6afc-4a3b-9181-49ba793b820f" />



<img width="792" height="245" alt="image" src="https://github.com/user-attachments/assets/5d246837-b071-45d8-ba4c-5ebd48af340f" />



Для брутфорса логин:пароль использовал эти почты и специальный вордлист https://standoff365.com/content/uploads/wordlist_standalone_c21df38645.txt


