# Установка PostgreSQL

Для корректной работы с командной строкой обновляем существующие пакеты:
> sudo apt update 

После этого можем устанавливать PostgreSQL используя следующую команду:
> sudo apt install postgresql posgtresql-contrib

Создаем учетную запись с помощью следующей команды:
> sudo -i -u postgres

Затем мы получаем доступ к командной строке Postgres с помощью команды:
> psql


# Установка pgAdmin

Для того, чтобы установить pgadmin необходимо получить файлы из интернета, используя команду curl получаем все необходимые пакеты и устанавливаем их в конфигурационный файл:
> curl https://www.pgadmin.org/static/packages_pgadmin_org.pub | sudo apt-key add
> sudo sh -c 'echo "deb https://ftp.postgresql.org/pub/pgadmin/pgadmin4/apt/$(lsb_release -cs) pgadmin4 main" > /etc/apt/sources.list.d/pgadmin4.list && apt update'

После чего можем запустить установку pgadmin:
>sudo apt install pgadmin4

Этой командой мы установили все необходимое для работы с pgadmin, следовательно для работы с ним необходимо включить возможность использования веб-интерфейса:
>sudo /usr/pgadmin4/bin/setup-web.sh

Далее мы можем работать с веб-интерфейсом pgAdmin перейдя по ссылке, полученной в терминале, в моем случае: 
http://127.0.0.1/pgadmin4
