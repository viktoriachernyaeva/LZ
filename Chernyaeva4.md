Табличные пространства
Создайте новую базу данных.Проверьте, какое табличное пространство по умолчанию установлено для новой базы данных.
Посмотрите в файловой системе символическую ссылкув PGDATA на каталог табличного пространства.
Удалите созданное табличное пространство.

Практика Установите параметр rando m_page_cost в значение 1.1для табличного пространства pg_default
Краткая информация:
* Табличные пространства — средство для организации физического хранения данных * Логическое (базы данных, схемы) и физическое (табличные пространства) разделения данных независимы
Задание 1
Создайте новую базу данных.Проверьте, какое табличное пространство по умолчанию установлено для новой базы данных.

=> CREATE DATABASE db;
=> SELECT spcname
FROM pg_tablespace
WHERE oid = (SELECT dattablespace FROM pg_database WHERE datname = 'db');

Табличное пространство по умолчанию — ts.
Вывод: табличное пространство по умолчанию определяется шаблоном, из которого клонируется новая база данных.
Результат: ![alt-текст](https://sun9-41.userapi.com/impg/QkCJE3N7KzvrcX8ekHy4iAaCcBwsuFZ2XKT12Q/ReLDt4dFPQA.jpg?size=495x206&quality=96&sign=833de8646a47bef73e4c951744378156&type=album "Текст заголовка логотипа 1")
Задание 2
Посмотрите в файловой системе символическую ссылкув PGDATA на каталог табличного пространства

=> SELECT oid AS tsoid FROM pg_tablespace WHERE spcname = 'ts';
student$ sudo ls -l /var/lib/postgresql/13/main/pg_tblspc/16703

Результат: ![alt-текст](https://sun9-54.userapi.com/impg/Pcv6dL6-zfife0uAo-28TaKDinMBkhuMnBjgRA/voPIkCQUS5g.jpg?size=489x67&quality=96&sign=7bdb2d7883955e8f47b346848d17c938&type=album "Текст заголовка логотипа 1")
### Задание 3 Удалите созданное табличное пространство.
```sh => ALTER DATABASE template1 SET TABLESPACE pg_default; => DROP DATABASE db; => DROP TABLESPACE ts; ```
Результат: alt-текст

Практика +
Получите список представлений в схеме information_schema.

=> SELECT name, setting
FROM pg_settings 
WHERE name IN ('seq_page_cost','random_page_cost');

Если используются диски с разными характеристиками, для них можно создать разные табличные пространства и настроить подходящие соотношения этих параметров. Например для быстрых SSD-дисков значение random_page_cost можно уменьшить практически до значения seq_page_cost.
```sh => ALTER TABLESPACE pg_default SET (random_page_cost = 1.1); ```
Настройки, сделанные командой ALTER TABLESPACE, сохраняются в таблице pg_tablespace. Их можно посмотреть в psql следующей командой:
```sh => \db+ ```
Параметры *_cost можно установить и в postgresql.conf. Тогда они будут действовать для всех табличных пространств.
Результат: ![alt-текст](https://sun2-11.userapi.com/impg/Gpyh-F8u4gQ9i9NhIn7IFNuZc3LPCCvN-4t64g/tEsmCIL0wao.jpg?size=485x302&quality=96&sign=8f08256e6e120478fca30871361e1fc6&type=album "Текст заголовка логотипа 1")