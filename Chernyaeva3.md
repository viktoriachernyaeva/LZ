# Создание ролей в PostgreSQL

Для создания ролей можно использовать два метода:

1. Используя учетную запись postgresql:
> sudo -i -u postgres

Этой командой мы входим в учетную запись. Далее в этом окне можем создавать роли:
> createuser --administrator

2. Используя sudo:
> sudo -u postgres createuser --administrator