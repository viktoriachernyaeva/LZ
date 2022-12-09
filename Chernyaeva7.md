Резервное копирование
Создайте базу данных и таблицу в ней с несколькими строками.
Сделайте логическую копию базы данных с помощью утилиты pg_dump. Удалите базу данных и восстановите ее из сделанной копии.
Сделайте автономную физическую резервную копию кластера с помощью утилиты pg_basebackup. Измените таблицу. Восстановите новый кластер из сделанной резервной копии и проверьте, что база данных не содержит более поздних изменений


##### Краткая информация:
* Логическая резервная копия —команды SQL для восстановления состояния объектов -команда copy, утилиты pg_dump и pg_dumpall * Физическая резервная копия —копия файлов кластера + набор файлов WAL -утилита pg_basebackup * Архив журнальных файлов -файловый или потоковый -позволяет восстановить систему на произвольный момент времен
Задание 1
Создайте базу данных и таблицу в ней с несколькими строками.


```sh => CREATE DATABASE backup_overview; => \c backup_overview => CREATE TABLE t(n integer); => INSERT INTO t VALUES (1), (2), (3); ```
Результат: ![alt-текст](https://sun9-64.userapi.com/impg/XyQg_-ZbFt7i1_v9_Z34-_dG-oevGZn30dCacw/KH5e45BqR9Q.jpg?size=434x100&quality=96&sign=ad049ece4b50a85fa70a09c254b413c8&type=album "Текст заголовка логотипа 1")
Задание 2
Сделайте логическую копию базы данных с помощью утилиты pg_dump. Удалите базу данных и восстановите ее из сделанной копии.


Создаем резервную копию и удаляем базу данных и восстанавливаем ее из копии:
```sh => \c postgres => DROP DATABASE backup_overview; student$ psql -f ~/backup_overview.dump => \c backup_overview => SELECT * FROM t; ```
Результат: ![alt-текст](https://sun9-56.userapi.com/impg/U9whX7N0kPYTWJUSwEOLsJopCBt-lr9NUnitTw/UVmT3d4ci_A.jpg?size=438x260&quality=96&sign=9367095f63d373e03a4cbe36c8b78859&type=album "Текст заголовка логотипа 1")
![alt-текст](https://sun9-72.userapi.com/impg/H7TgdJkKYM1MuHA_QL6hPNlGV8VDSJm1pj1s0w/EWCDVgbQ3vM.jpg?size=435x265&quality=96&sign=dfcdef9b6fbc140d1a2ca5dc6a7ae9ef&type=album "Текст заголовка логотипа 2")
![alt-текст](https://sun9-25.userapi.com/impg/s5oHqKNiDabYkkkHw1TbWPoGuD9MZbAwOA6agw/iH9a0z6B24Y.jpg?size=438x114&quality=96&sign=0169e5ab395fe8ee4125d4f37248e656&type=album "Текст заголовка логотипа 3")
Задание 3
Сделайте автономную физическую резервную копию кластера с помощью утилиты pg_basebackup. Измените таблицу. Восстановите новый кластер из сделанной резервной копии и проверьте, что база данных не содержит более поздних изменений


student$ sudo rm -rf /home/student/basebackup
student$ pg_basebackup --pgdata=/home/student/basebackup
---Убеждаемся, что второй сервер остановлен, и выкладываем резервную копию:---
student$ sudo pg_ctlcluster 13 replica status
student$ sudo rm -rf /var/lib/postgresql/13/replica
student$ sudo mv /home/student/basebackup/ /var/lib/postgresql/13/replica
student$ sudo chown -R postgres:postgres /var/lib/postgresql/13/replica
---Изменяем таблицу:---
=> DELETE FROM t;
---Запускаем сервер из резервной копии:---
student$ sudo pg_ctlcluster 13 replica start
student$ /usr/lib/postgresql/13/bin/psql -p 5433 -d backup_overview
=> SELECT * FROM t;

Результат: ![alt-текст](https://sun9-13.userapi.com/impg/3I4oi1IOnWL0-HftsWMCWj4cEYbcle4qu-hY3A/KJrv1CVF5Ns.jpg?size=435x157&quality=96&sign=0304dfb18025f015a5086979d6e45356&type=album "Текст заголовка логотипа 1")
Практика +
Настройте аутентификацию, лишенную этого недостатка. Убедитесь в правильности изменений


Сохраним исходный файл настроек:
```sh student$ sudo cp -n /etc/postgresql/13/main/pg_hba.conf ~/pg_hba.conf.orig ```
Будем управлять аутентификацией пользователей, добавляя их в группу locals. Запишем файл pg_hba.conf с нуля:
```sh student$ sudo tee /etc/postgresql/13/main/pg_hba.conf << EOF local all student trust local all +locals trust EOF student$ sudo pg_ctlcluster 13 main reload => CREATE ROLE locals; ```
Проверка
```sh => CREATE ROLE alice LOGIN; => GRANT locals TO alice; => CREATE ROLE bob LOGIN; student$ psql "dbname=student user=alice" -c student$ psql "dbname=student user=bob" -c "\conninfo""\conninfo" => GRANT locals TO bob; student$ psql "dbname=student user=bob" -c "\conninfo" ---Восстановление исходных настроек--- student$ sudo cp ~/pg_hba.conf.orig /etc/postgresql/13/main/pg_hba.conf student$ sudo pg_ctlcluster 13 main reload ```