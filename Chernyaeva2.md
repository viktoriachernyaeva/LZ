# Установка PostgreSQL из исходных кодов

Для установки PostgreSQL можно использовать еще один способ, который заключается в установке архива с исходными кодами. Этот способ будет описан ниже.

Обновляем пакеты на устройстве следующими командами:

> sudo apt upgrade && apt update

И установим необходимые для работы пакеты:

> * dnf install readline-devel
> * dnf install zlib-devel
> * dnf install gcc
> * dnf install make

После чего можем приступать к установке, для чего открываем официальный сайт PostgreSQL => Загрузки => Исходные коды.
Выбираем последнюю версию Postgres и устанавливаем ее на устройство. Далее распаковываем установленный архив следующим образом: 
> gunzip postgresql-&&.&.tar.gz


> tar -xvf postgresql-&&.&.tar

## Конфигурирование и установка на устройство

Для конфигурирования пакетов, и дальнейшей настройки используем следующие команды в указанной последовательности:

>/configure --prefix=/local/apps/postgresql/pgsql115/ --with-pgport=5432
make  		<= начало установки

>make install    <= установка postgresql в конфигурационный файл

>useradd -d /home/postgres/ postgres <= добавляем пользователя

>passwd postgres <= устанавливаем пароль


>mkdir -p /local/apps/postgresql/pgsql???/data <= создаем папку с данными для postgres


>chown postgres /local/apps/postgresql/pgsql???/data

>su - postgres
/local/apps/postgresql/pgsql???/bin/initdb -D /local/apps/postgresql/pgsql???/data
/local/apps/postgresql/pgsql???/bin/postgres -D /local/apps/postgresql/pgsql???/data

>ps -ef | grep postgres

>vi .bash_profile
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/local/apps/postgresql/pgsql???/lib
export PATH=$PATH:/local/apps/postgresql/pgsql???/bin