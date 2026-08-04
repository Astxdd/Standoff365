# Corp-wiki

Корпоративная вики компании «ОффЭнерго»
Инженеры компании «ОффЭнерго» готовы презентовать новейшую разработку в области генерации электроэнергии. Эта технология важна для Государства F и не должна попасть в руки конкурентов. Поэтому ее описание надежно хранится на корпоративной вики компании. Разве может что-то пойти не так?
Хост: corp-wiki.standalone.stf 10.124.249.

для начало нмапим айпишник:
<img width="825" height="275" alt="image" src="https://github.com/user-attachments/assets/99eba7bb-484d-4f76-b509-0bce5f9321f5" />

видим порты 80, 3306 и 8082

на 80 порте у нас вики корпорации

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c1f92ab1-4479-408a-b127-d661f434270a" />


в статьях особо ничего нет кроме никнеймов admin и emivil 

так же есть логин и регистрация, для регистрации нужен код приглашения, а для логина у нас нет кред, только юзернеймы

пробуем брутить пароли admin и emivil

у последнего я смог сбрутить пароль, он был 1234

зашел на его аккаунт, нашел новую статью с паролем для mysql

креды: roblee:;;48~Various~Boat~Cold~97;;

заходим по 3306 порту на mysql

mariadb -h 10.124.249.9 -P 3306 -u roblee -p --ssl-verify-server-cert=OFF

<img width="807" height="413" alt="image" src="https://github.com/user-attachments/assets/f9486b00-603c-4a5f-ace4-a86ac28055d9" />


заходим в wiki и добавляем свой пригласительный код 

<img width="820" height="323" alt="image" src="https://github.com/user-attachments/assets/97245fb3-b7e7-43ce-b43c-6003159d5a15" />

теперь можно зарегать своего юзера
зайдя на свой профиль теперь можно увидить три функции
<img width="1280" height="364" alt="image" src="https://github.com/user-attachments/assets/0e961644-c59b-4d71-9796-990fae0cedc8" />

самая интересная фукнция это edit avatar, так как она требует link to avatar, то есть возможный ssrf
<img width="1280" height="364" alt="image" src="https://github.com/user-attachments/assets/73d45d07-7bd4-4539-ac61-0f4157ac23c5" />

при file:///etc/passwd нам выдает это:

<img width="799" height="175" alt="image" src="https://github.com/user-attachments/assets/fe9d67ac-9b52-4d86-9919-867a9ca1ad09" />

в бурпе доводим дело до флага ssrf:

<img width="1554" height="636" alt="image" src="https://github.com/user-attachments/assets/bef5f108-c3b6-4a6d-a709-61efe088d411" />


поискав еще статьей нахожу http://searchserver:8000/api/data

доводим скулю чтобы взять креды админа:
<img width="1554" height="636" alt="image" src="https://github.com/user-attachments/assets/a752e894-f2b0-4a50-85c5-a0b3e1a88b1a" />

логинимся и в статье "Proprietary Formula: HyperFusionX" находим флаг:


<img width="1920" height="903" alt="image" src="https://github.com/user-attachments/assets/d1ed74ce-362d-46db-a4ad-4da14f73f8cb" />


на этом всё, по скорборду видно, что есть вторая бага на 200, но я не смог ее найти (

<img width="1837" height="956" alt="image" src="https://github.com/user-attachments/assets/f8ec9bd6-8ef5-4e52-ad80-f4bb0e43e44e" />

<img width="1920" height="903" alt="image" src="https://github.com/user-attachments/assets/fa9611f5-d1b7-4168-b64d-9f8c6cda9304" />





