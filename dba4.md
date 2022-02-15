#### Создаем виртуалку. Прописываем репозиторий с 13 postgresql. Устанавливаем postgres
#### подключаемся к кластеру
        postgres=# \l
                                      List of databases
           Name    |  Owner   | Encoding | Collate |  Ctype  |   Access privileges   
        -----------+----------+----------+---------+---------+-----------------------
         postgres  | postgres | UTF8     | C.UTF-8 | C.UTF-8 | 
         template0 | postgres | UTF8     | C.UTF-8 | C.UTF-8 | =c/postgres          +
                   |          |          |         |         | postgres=CTc/postgres
         template1 | postgres | UTF8     | C.UTF-8 | C.UTF-8 | =c/postgres          +
                   |          |          |         |         | postgres=CTc/postgres
        (3 rows)
#### создаем БД
        postgres=# create database testdb;
        CREATE DATABASE
#### Создаем схему testnm и случайно создают таблицу без указания схемы (попадает в public). Создаем таблицу в схеме
        testdb=# create table testnm.t1 (c1 integer);
        CREATE TABLE
#### Вставляем данные (1 строку)
        testdb=# insert into testnm.t1 values (1);
        INSERT 0 1
#### Создаем роль и даем права по подключение к тестовой БД
        testdb=# create role readonly;
        CREATE ROLE
        testdb=# GRANT CONNECT ON DATABASE testdb TO readonly;
        GRANT
#### Грантуем и создаем пользователя
        testdb=# GRANT USAGE ON SCHEMA testnm TO readonly;
        GRANT
        testdb=# GRANT SELECT ON ALL TABLES IN SCHEMA testnm TO readonly;
        GRANT
        testdb=# create user test123 with encrypted password '1234';
        CREATE ROLE
        testdb=# GRANT readonly to test123;
        GRANT ROLE
#### Запускаем отдельную консоль и пробуем зайти пользователем. Вроде как получилось. Даже не пришлось pg_hba править
        dklimovich@dba4logic:~$ psql -U test123 -h localhost testdb
        Password for user test123: 
        psql (13.6 (Ubuntu 13.6-1.pgdg20.04+1))
        SSL connection (protocol: TLSv1.3, cipher: TLS_AES_256_GCM_SHA384, bits: 256, compression: off)
        Type "help" for help.
        testdb=> select * from testnm.t1;
         c1 
        ----
          1
        (1 row)
#### видимо сразу создал и в схеме и без. К таблице в схеме public доступа у пользователя нет
        testdb=> select * from public.t1;
        ERROR:  permission denied for table t1
#### Пытаемся создать таблицу под пользователем и вставить туда данные. Получилось
        testdb=> create table t2(c1 integer);
        CREATE TABLE
        testdb=> insert into t2 values (2);
        INSERT 0 1
#### Запрашиваем права пользователя test123. Он довольно могуч в схеме public
        testdb=# SELECT * FROM information_schema.table_privileges where grantee = 'test123';
         grantor | grantee | table_catalog | table_schema | table_name | privilege_type | is_grantable | with_hierarchy 
        ---------+---------+---------------+--------------+------------+----------------+--------------+----------------
         test123 | test123 | testdb        | public       | t2         | INSERT         | YES          | NO
         test123 | test123 | testdb        | public       | t2         | SELECT         | YES          | YES
         test123 | test123 | testdb        | public       | t2         | UPDATE         | YES          | NO
         test123 | test123 | testdb        | public       | t2         | DELETE         | YES          | NO
         test123 | test123 | testdb        | public       | t2         | TRUNCATE       | YES          | NO
         test123 | test123 | testdb        | public       | t2         | REFERENCES     | YES          | NO
         test123 | test123 | testdb        | public       | t2         | TRIGGER        | YES          | NO
        (7 rows)
#### Пробую отозвать права на схему public у роли readonly. Неуспешно. Создание таблиц работает
        REVOKE CREATE ON SCHEMA public FROM readonly;
        testdb=> create table t3(c1 integer);
        CREATE TABLE      
#### Отзываю права всем пользователем кроме админов и владельцев 
        testdb=# REVOKE CREATE ON SCHEMA public FROM PUBLIC;
        REVOKE
#### Успешно. Больше таблицу создать нельзя
        testdb=> create table t4(c1 integer);
        ERROR:  permission denied for schema public
#### все изменения прав делал из отдельной консоли под postgres
