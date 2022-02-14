#### Ставим docker
        curl -fsSL https://get.docker.com -o get-docker.sh
        sudo sh get-docker.sh
#### Создаем сеть для контейнеров
        dklimovich@docker:~$ sudo docker network create netpg
        66ae00df31962a66c6dc6c0d6c0e2f6da880da503e6ef7ecf5a8db9a1a419ae0
#### Запускаем контейнер с 13 postgres
        dklimovich@docker:~$ sudo docker run --name pg-docker --network netpg -e POSTGRES_PASSWORD=postgres -d -p 5432:54
        32 -v /var/lib/postgres:/var/lib/postgresql/data postgres:13
        Unable to find image 'postgres:13' locally
        13: Pulling from library/postgres
        5eb5b503b376: Pull complete 
        daa0467a6c48: Pull complete 
        7cf625de49ef: Pull complete 
        bb8afcc973b2: Pull complete 
        c74bf40d29ee: Pull complete 
        2ceaf201bb22: Pull complete 
        1255f255c0eb: Pull complete 
        12a9879c7aa1: Pull complete 
        9852c24f0019: Pull complete 
        0af9460e4682: Pull complete 
        021fda0747bb: Pull complete 
        36b3aa5da47e: Pull complete 
        cf87929fe7e1: Pull complete 
        Digest: sha256:4c1152fba3fee3b378a2231f7f6f2bfc209d8f6f81f93ad9dab3bb708abed00f
        Status: Downloaded newer image for postgres:13
        5714c33f8663bdb8630e85bdd162d20bb66536f3a7fdf4a6b65e24099be548e3
#### Подключаемся к контейнеру postgres и создаем базовое наполнение БД
        dklimovich@docker:~$ sudo docker run -it --rm --network netpg --name pg-client postgres:13 psql -h pg-docker -U p
        ostgres
        Password for user postgres: 
        psql (13.6 (Debian 13.6-1.pgdg110+1))
        Type "help" for help.
        postgres=# create table dba4 (id int, name text);
        CREATE TABLE
        postgres=# insert into dba4 (id, name) values (1, 'Dmitriy'), (2, 'Ivan'), (3,'Petr');
        INSERT 0 3
        postgres=# select id, name from dba4 ;
         id |  name   
        ----+---------
          1 | Dmitriy
          2 | Ivan
          3 | Petr
        (3 rows)
        postgres=# 
