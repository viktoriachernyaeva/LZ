Создание ролей
Создайте роль creator без права входа в систему,но с правом создания баз данных и ролей. Создайте пользователя weak с правом входа в систему.
Убедитесь, что weak не может создать базу данных.
Включите пользователя weak в группу creator. Создайте новую базу данных под пользователем weak
Создайте роли VIKA и admin с правом входа в систему. Создайте таблицу под ролью VIKA.
Сделайте необходимые настройки так, чтобы обе роли могли изменять структуру таблицы (например, добавлять столбцы командой ALTER TABLE)
Краткая информация:
Атрибуты определяют свойства ролиCREATE ROLE роль [WITH] атрибут [атрибут ...]
Удалить роль можно, если нет объектов, которыми она владеет. DROP ROLE
Перед удалением роли можно передать ее объекты другой роли: REASSIGN OWNED BY (удаляемая_роль) TO (принимающая_роль);
Роли объединяют концепции пользователей и группАтрибуты определяют свойства ролей
Роли можно включать друг в друга
У каждого объекта базы данных есть роль-владелец
Задание1
Создайте роль creator без права входа в систему,но с правом создания баз данных и ролей. Создайте пользователя weak с правом входа в систему

postgres=# CREATE ROLE VIKA WITH LOGIN;
postgres=# CREATE ROLE admin WITH LOGIN;

Результат: ![alt-текст]![Image](https://user-images.githubusercontent.com/113884036/206639255-33e63d4f-998f-4f48-92c3-12095f642aed.png)
 "Текст заголовка логотипа 1")
Далее создаем таблицу под ролью VIKA:
### Задание2 Убедитесь, что weak не может создать базу данных.
```sh postgres=# \c - weak postgres=# CREATE DATABASE access_roles; ```
Результат: ![alt-текст](https://sun9-36.userapi.com/impg/_kG-t2NMPZIDlfJ3u7maTEVBW4c1kTry6Y5Wpw/V6Zn66Kv-p4.jpg?size=493x82&quality=96&sign=7a97b8870fbf02144635c0cbae738688&type=album "Текст заголовка логотипа 1")
### Задание3 Включите пользователя weak в группу creator. Создайте новую базу данных под пользователем weak
```sh postgres=# \c - weak postgres= SET ROLE creator; postgres=> CREATE DATABASE ars; ```
Результат: ![alt-текст](https://sun9-31.userapi.com/impg/wiUNUXQ0DTNzyTyDpdBu34RscqfzDO-VBA6cEA/qAtPbkHLj2I.jpg?size=492x159&quality=96&sign=866964216eb2cf1ac6935fa1ab1e602f&type=album "Текст заголовка логотипа 1")
### Задание4 Создайте роли VIKA и admin с правом входа в систему. Создайте таблицу под ролью VIKA.
Чтобы создать необходимые нам роли вводим следущие команды:
```sh postgres=# CREATE ROLE VIKA WITH LOGIN; postgres=# CREATE ROLE admin WITH LOGIN; ```
Результат: ![alt-текст]![Image](https://user-images.githubusercontent.com/113884036/206640104-2bbfb921-2fdf-43a6-b69c-9bf7cd7c5a5a.jpg)

 "Текст заголовка логотипа 1")
Далее создаем таблицу под ролью VIKA:
```sh postgres=# \c access_roles VIKA access_roles=> CREATE TABLE test (id integer); ```
Результат: ![alt-текст]![Image]![Image](https://user-images.githubusercontent.com/113884036/206642010-4bbd9a52-4886-473d-bca1-329fad6f938b.jpg)

 "Текст заголовка логотипа 1")
Задание5
Добавление владельца таблицы

Чтобы admin мог изменять структуру таблицы, он должен стать ее владельцем. Для этого можно включить роль admin в роль VIKA. Такую команду может выполнить VIKA:
```sh access_roles=> GRANT VIKA TO admin; access_roles=> \du VIKA|admin ```
![alt-текст] "Текст заголовка логотипа 1")
Теперь admin может добавить столбец:

access_roles=> \c - admin 
access_roles=> ALTER TABLE test ADD description text;

Результат: ![alt-текст](https://sun9-8.userapi.com/impg/oGsLRPuqayiOHo1EScwNFREB_WnQn0wq8ip-yg/sZFy1ZCZDx4.jpg?size=492x77&quality=96&sign=80386e9953e3b9e4c620959e75fd3dfb&type=album "Текст заголовка логотипа 1")
И даже удалить таблицу: ```sh access_roles=> DROP TABLE test; ```
Результат: ![alt-текст](https://sun9-79.userapi.com/impg/QV0AQIwN8cAmRJvViODGCbOPh8cfPRcMAhk0yA/QO6fbdQIYdc.jpg?size=488x46&quality=96&sign=ba8a9243082d22d4766f255e0f5f945d&type=album "Текст заголовка логотипа 1")