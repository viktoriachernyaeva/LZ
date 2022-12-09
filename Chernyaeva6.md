Системный_каталог

Задание 1
Получите описание таблицы pg_class.

=> \d pg_class

Результат: ![alt-текст](https://sun9-46.userapi.com/impg/Dza9r_ljSUBsVqpzc477HlTTHi26DBC4aWohzw/2jR9-OnIxRo.jpg?size=732x407&quality=96&sign=3e849970458aae57182481c2c11ebe9c&type=album "Текст заголовка логотипа 1")
Задание 2
Получите подробное описание представления pg_tables.

=> \d+ pg_tables

Результат: ![alt-текст](https://sun9-39.userapi.com/impg/QnUiHiFJAnV5PQ_ulSiPn1MQZtccju6yhPDvuQ/fymmo2S6_0g.jpg?size=732x344&quality=96&sign=6ddff9dabb0d78a02c39ed0a4874ad1d&type=album "Текст заголовка логотипа 1")
### Задание 3 Создайте базу данных и временную таблицу в ней. Получите полный список схем в базе, включая системные.
```sh => CREATE DATABASE data_catalog; => \c data_catalog => CREATE TEMP TABLE t(n integer); => \dnS ```
Временная таблица расположена в схеме pg_temp_N, где N — некоторое число. Такие схемы создаются для каждого сеанса, в котором появляются временные объекты, поэтому их может быть несколько. Имя схемы для временных объектов текущего сеанса можно получить, обратившись к системной функции:
```sh => SELECT pg_my_temp_schema()::regnamespace; ```
Однако в большинстве случаев точное имя схемы знать не нужно, поскольку при необходимости к временному объекту можно обратиться, используя имя схемы pg_temp:
```sh => SELECT * FROM pg_temp.t; ```
Предназначение некоторых других схем нам уже известно, а с оставшимися (pg_toast*) познакомимся позже.
Результат: ![alt-текст](https://sun9-8.userapi.com/impg/sxRq8-MKh5xd_XjyIFzqYrqQC4VPXMH8-A3CxQ/zZ8vPz-zjus.jpg?size=730x397&quality=96&sign=ef4e6e363b8d5ebf3b1a9dac41c1c725&type=album "Текст заголовка логотипа 1") ### Задание 4 Получите список представлений в схеме information_schema.
```sh => \dv information_schema.* ```
Результат: ![alt-текст](https://sun9-1.userapi.com/impg/0nvdbPP4vSiDd4ogNwHXOrzk6pUY3HL7PHJajA/odmN9WO4pQw.jpg?size=739x400&quality=96&sign=5f69b21aa708bf1d17412ad66292bce2&type=album "Текст заголовка логотипа 1")
Задание 5

Какие запросы выполняет следующая команда psql?\d+ pg_views
Чтобы увидеть запросы, которые выполняют команды psql, включим переменную ECHO_HIDDEN.
```sh => \set ECHO_HIDDEN on => \d+ pg_views => \set ECHO_HIDDEN off ```
![alt-текст](https://sun9-42.userapi.com/impg/CzY7qRV-MnS7QH09BcEceTq8cwt08XHdYpB1Xg/dEx31hN1aDc.jpg?size=730x386&quality=96&sign=48784512b670c40bbd37d8fc02f18c48&type=album "Текст заголовка логотипа 1")
![alt-текст](https://sun9-58.userapi.com/impg/HAC5GQgsumrV8J3nbrhxCWCrPN5BanuFNuASrQ/JahIHaMyfBw.jpg?size=730x381&quality=96&sign=4ba21740d36832d75187b3be923aa9a8&type=album "Текст заголовка логотипа 1")
![alt-текст](https://sun9-40.userapi.com/impg/GInmVUMpHPqWCOmhgKhmRuE928DY4PL60njyIg/zzHvwGS_OfA.jpg?size=730x215&quality=96&sign=5b3e643c12443fdb67c01880046ba62e&type=album "Текст заголовка логотипа 1")