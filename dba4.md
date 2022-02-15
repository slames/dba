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
