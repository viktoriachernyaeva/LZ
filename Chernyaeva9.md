Низкий уровень

Задание 1
Создайте новую базу данных.Проверьте, какое табличное пространство по умолчанию установлено для новой базы данных.

student$ sudo mkdir /var/lib/postgresql/ts_dir
student$ sudo chown postgres /var/lib/postgresql/ts_dir
=> CREATE TABLESPACE ts LOCATION '/var/lib/postgresql/ts_dir';
=> CREATE DATABASE data_lowlevel;
=> \c data_lowlevel
=> CREATE UNLOGGED TABLE u(n integer) TABLESPACE ts;
=> INSERT INTO u(n) SELECT n FROM generate_series(1,1000) n;
=> SELECT pg_relation_filepath('u');

Посмотрим на файлы таблицы.
Обратите внимание, что следующая команда ls выполняется от имени пользователя postgres. Чтобы повторить такую команду, удобно сначала открыть еще одно окно терминала и переключиться в нем на другого пользователя командой:
```sh student$ sudo su postgres postgres$ ls -l /var/lib/postgresql/13/main/pg_tblspc/16705/PG_13_202007201/16706/16707* => DROP TABLE u; => DROP TABLESPACE ts; ```
Результат: ![alt-текст](https://sun9-18.userapi.com/impg/SDD-FUeFjkwh2xMxrjs6WuHwLQJVHZqMjbW-CA/tR5L2TJnZCo.jpg?size=525x302&quality=96&sign=a91d367dda93de3ed8329b367064535f&type=album "Текст заголовка логотипа 1")
Задание 2
Посмотрите в файловой системе символическую ссылкув PGDATA на каталог табличного пространства

=> CREATE TABLE t(s text);
=> \d+ t

По умолчанию для типа text используется стратегия extended.
Изменим стратегию на external:

=> ALTER TABLE t ALTER COLUMN s SET STORAGE external;
=> INSERT INTO t(s) VALUES ('Короткая строка.');
=> INSERT INTO t(s) VALUES (repeat('A',3456));
=> SELECT relname FROM pg_class WHERE oid = (
  SELECT reltoastrelid FROM pg_class WHERE relname='t'
);

=> SELECT chunk_id, chunk_seq, length(chunk_data)
FROM pg_toast.pg_toast_16710
ORDER BY chunk_id, chunk_seq;

Видно, что в TOAST-таблицу попала только длинная строка (два фрагмента, общий размер совпадает с длиной строки). Короткая строка не вынесена в TOAST просто потому, что в этом нет необходимости — версия строки и без этого помещается в страницу.
Результат: ![alt-текст](https://sun9-9.userapi.com/impg/_2xDlwWyqpR_T1Gsvo_oeWdlfamgpVdv6tC0ig/QSsutxKYTdk.jpg?size=525x158&quality=96&sign=a0292358072d265c5098f31a51908cb2&type=album "Текст заголовка логотипа 1")
![alt-текст](https://sun9-49.userapi.com/impg/pqh_j87YzehdiV2rkXrWsHBGXNgI0MnY_03GuQ/nUklbBCY9r8.jpg?size=434x245&quality=96&sign=52251b6a38ccd769aa65b6d3d3db581a&type=album "Текст заголовка логотипа 1")
Практика +
Создайте базу данных. Сравните размер базы данных, возвращаемый функцией pg_database_size, с общим размеров всех таблиц в этой базе
Список таблиц базы данных можно получить из таблицы pg_class системного каталога
Сравнение размеров базы данных и таблиц в ней

=> CREATE DATABASE data_lowlevel;
=> \c data_lowlevel

Даже пустая база данных содержит таблицы, относящиеся к системного каталогу. Полный список отношений можно получить из таблицы pg_class. Из выборки надо исключить: * таблицы, общие для всего кластера (они не относятся к текущей базе данных); * индексы и toast-таблицы (они будут автоматически учтены при подсчета размера).
```sh => SELECT sum(pg_total_relation_size(oid)) FROM pg_class WHERE NOT relisshared -- локальные объекты базы AND relkind = 'r'; -- обычные таблицы => SELECT pg_database_size('data_lowlevel'); ```
Размер базы данных оказывается несколько больше.
Дело в том, что функция pg_database_size возвращает размер каталога файловой системы, а в этом каталоге находятся несколько служебных файлов.
```sh => SELECT oid FROM pg_database WHERE datname = 'data_lowlevel'; ```
Обратите внимание, что следующая команда ls выполняется от имени пользователя postgres. Чтобы повторить такую команду, удобно сначала открыть еще одно окно терминала и переключиться в нем на другого пользователя командой:
```sh student$ sudo su postgres postgres$ ls -l /var/lib/postgresql/13/main/base/16717/[^0-9]* ```
* pg_filenode.map — отображение oid некоторых таблиц в имена файлов; * pg_internal.init — кеш системного каталога; * PG_VERSION — версия PostgreSQL.
Из-за того, что одни функции работают на уровне объектов базы данных, а другие — на уровне файловой системы, бывает сложно точно сопоставить возвращаемые размеры. Это относится и к функции pg_tablespace_size.
Результат: ![Image](https://user-images.githubusercontent.com/113884036/206674193-68a2c6da-4fc1-4db9-a998-69b9575e1c21.png)
alt-текст