Привилегии
Настройте привилегии таким образом, чтобы одни пользователи имели полный доступ к таблицам, а другие могли только запрашивать, но не изменять информацию.

Создайте новую базу данных и две роли: writer и reader.
Отзовите у роли public все привилегии на схему public,выдайте роли writer обе привилегии, а роли reader — только usage.
Настройте привилегии по умолчанию так, чтобы роль reader получала доступ на чтение к таблицам, принадлежащим writerв схеме public.
Создайте пользователей w1 в группе writer и r1 в группе reader.
Под пользователем writer создайте таблицу.
Убедитесь, что r1 имеет доступ к таблице только на чтение,а w1 имеет к ней полный доступ, включая удаление.

### Практика+1. 7. Создайте роль alice. Создайте таблицу. Выдайте роли alice привилегию на чтение таблицыи привилегию на изменение с правом передачи. 8. Проверьте права доступа к созданной таблице с помощью представления table_privileges информационной схемы.Сравните с выводом команды \dp. 9. Проверьте права доступа к созданной таблице с помощью функции has_table_privileges
##### Краткая информация: Привилегии определяют права доступа ролей к объектам
Задание 1
Создайте новую базу данных и две роли: writer и reader.

postgres=# CREATE DATABASE access_privileges;
postgres=# CREATE USER writer;
postgres=# CREATE USER reader;

Результат: ![alt-текст](https://sun9-79.userapi.com/impg/bR1pI0TwPRY0wN5mPnXYO-tzwDYQXyHJBccvHQ/5gt4_5YQ7gs.jpg?size=491x96&quality=96&sign=e045dbe26d41eede6bfbea9ff756be0a&type=album "Текст заголовка логотипа 1")
Далее создаем таблицу под ролью VIKA:
### Задание 2 Отзовите у роли public все привилегии на схему public,выдайте роли writer обе привилегии, а роли reader — только usage.
```sh postgres=# \c access_privileges access_privileges=# REVOKE ALL ON SCHEMA public FROM public; access_privileges=# GRANT ALL ON SCHEMA public TO writer; access_privileges=# GRANT USAGE ON SCHEMA public TO reader; ```
Результат: ![alt-текст](https://sun9-73.userapi.com/impg/JHN49bQDtK2br9cVvebBgcN167y1kLmUKygNbg/Ipqzmmk3V88.jpg?size=492x124&quality=96&sign=e482b90366ab62d7128dc70c1941b686&type=album "Текст заголовка логотипа 1")
### Задание 3 Настройте привилегии по умолчанию так, чтобы роль reader получала доступ на чтение к таблицам, принадлежащим writerв схеме public.
```sh access_privileges=# ALTER DEFAULT PRIVILEGES FOR ROLE writer IN SCHEMA public GRANT SELECT ON TABLES TO reader; ```
Результат: ![alt-текст](https://sun9-45.userapi.com/impg/TkZsPONfbYUNjmsDFY_1E6iywtgu0JR553OmDw/ANiF7coSlTA.jpg?size=491x85&quality=96&sign=748635770ad9eb0191e6cb9695325c9d&type=album "Текст заголовка логотипа 1")
### Задание 4 Создайте пользователей w1 в группе writer и r1 в группе reader.
Чтобы создать необходимые нам роли вводим следущие команды:
```sh access_privileges=# CREATE ROLE w1 LOGIN IN ROLE writer; ```
Результат: ![alt-текст](https://sun9-32.userapi.com/impg/f17cHjg-X6rnk1nwKtg8NjeiL_P4j0zX5DD0KQ/1TjdXz0tzbY.jpg?size=490x31&quality=96&sign=e91ea050d1d9990f55f21d334723c018&type=album "Текст заголовка логотипа 1")
Конструкция IN ROLE сразу же добавляет новую роль в указанную. То есть такая команда эквивалентна двум:
```sh access_privileges=# CREATE ROLE w1 LOGIN; access_privileges=# GRANT writer TO w1; ```
Читающая роль:
```sh access_privileges=# CREATE ROLE r1 LOGIN IN ROLE reader; ```
Результат: ![alt-текст](https://sun9-20.userapi.com/impg/d1Y8zCeRg7_CtxFJ6UDpNBeN4DQ-UVuqyV7T2g/0hhOdPHmVKM.jpg?size=494x42&quality=96&sign=a9539f537fb08b94527418663fc4eb10&type=album "Текст заголовка логотипа 1")
Задание 5

Под пользователем writer создайте таблицу.
```sh access_privileges=# \c - writer access_privileges=# CREATE TABLE t(n integer); ```
![alt-текст](https://sun9-9.userapi.com/impg/n-RAbDH08Jz5OEyjK88BxxEELMCpBURB9K-pHw/hYrszoAlvnA.jpg?size=490x69&quality=96&sign=a9003acd160733965ded2abe529c8632&type=album "Текст заголовка логотипа 1")
Задание 6

Убедитесь, что r1 имеет доступ к таблице только на чтение,а w1 имеет к ней полный доступ, включая удаление.
```sh access_privileges=# \c - w1 access_privileges=> INSERT INTO t VALUES (42); ``` Роль r1 может читать таблицу:
```sh access_privileges=> \c - r1 access_privileges=> SELECT * FROM t; ```
Но не может изменить:
```sh access_privileges=> UPDATE t SET n = n + 1; ```
Роль w1 может удалить таблицу:
```sh access_privileges=> \c - w1 access_privileges=> DROP TABLE t; ```
![alt-текст](https://sun9-59.userapi.com/impg/5cYIL2kPR0LWT7duSo07pdtOTOS25IxjjO-5NA/OSME2Cd-M9s.jpg?size=490x315&quality=96&sign=ee536e9aec03ef8d6f7a7e9d393c4863&type=album "Текст заголовка логотипа 1")
В PostgreSQL 14 будет доступна преднастроенная роль pg_read_all_data, автоматически дающая возможность чтения любых данных.

Задание 7

Создайте роль alice. Создайте таблицу. Выдайте роли alice привилегию на чтение таблицыи привилегию на изменение с правом передачи.
```sh access_privileges=# CREATE DATABASE access_privileges; access_privileges=# CREATE USER alice; access_privileges=# \c access_privileges access_privileges=# CREATE TABLE test(id integer); access_privileges=# GRANT SELECT ON test TO alice; access_privileges=# GRANT UPDATE ON test TO alice WITH GRANT OPTION; ```
alt-текст

Задание 8

Проверьте права доступа к созданной таблице с помощью представления table_privileges информационной схемы.Сравните с выводом команды \dp.
```sh access_privileges=# SELECT grantee, grantor, privilege_type, is_grantable FROM information_schema.table_privileges WHERE table_name = 'test'; access_privileges=# \du lubov|admin access_privileges=# \dp test ``` Команда psql показывает ту же информацию, но в другом виде.
![alt-текст](https://sun9-84.userapi.com/impg/27e5C2ZDGiKFaSLOO9wH8Cqq-185o3hCQLPNVg/YhOBEWKwXQM.jpg?size=492x354&quality=96&sign=a8a8eeacb0279861ba40add395daaae7&type=album "Текст заголовка логотипа 1")
Задание 9

Проверьте права доступа к созданной таблице с помощью функции has_table_privileges

```sh access_privileges=#SELECT has_table_privilege('alice', 'test', 'SELECT') AS has_select, has_table_privilege('alice', 'test', 'UPDATE') AS has_update, has_table_privilege('alice', 'test', 'DELETE') AS has_delete; ```
![alt-текст](https://sun9-49.userapi.com/impg/2pq7BaYKi2CkyUSK6SPMOt3fjn6VT-BfiJwOKw/i3CMbivOngs.jpg?size=492x145&quality=96&sign=d507a885c6dc6cdb63104aaa85083735&type=album "Текст заголовка логотипа 1")