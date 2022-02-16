#### Ставим postgresql. В данном случае 14ый
#### Настраиваем логирование блокировок
        Устанавливаем log_lock_waits = on и
        deadlock_timeout = 200  #миллисекунд
#### Перезапускаем кластер. Открываем несколько транзакций и смотрим результат блокировок
        postgres=# select * from pg_locks;
           locktype    | database | relation | page | tuple | virtualxid | transactionid | classid | objid | objsubid | virtualtransaction | pid  |       mode       | granted | fastpath | waitstart 
        ---------------+----------+----------+------+-------+------------+---------------+---------+-------+----------+--------------------+------+------------------+---------+----------+-----------
         relation      |    13726 |    16384 |      |       |            |               |         |       |          | 5/27               | 6828 | RowExclusiveLock | t       | t        | 
         virtualxid    |          |          |      |       | 5/27       |               |         |       |          | 5/27               | 6828 | ExclusiveLock    | t       | t        | 
         relation      |    13726 |    16384 |      |       |            |               |         |       |          | 4/25               | 6814 | RowExclusiveLock | t       | t        | 
         virtualxid    |          |          |      |       | 4/25       |               |         |       |          | 4/25               | 6814 | ExclusiveLock    | t       | t        | 
         relation      |    13726 |    12290 |      |       |            |               |         |       |          | 3/13               | 6464 | AccessShareLock  | t       | t        | 
         virtualxid    |          |          |      |       | 3/13       |               |         |       |          | 3/13               | 6464 | ExclusiveLock    | t       | t        | 
         transactionid |          |          |      |       |            |           736 |         |       |          | 4/25               | 6814 | ExclusiveLock    | t       | f        | 
         transactionid |          |          |      |       |            |           737 |         |       |          | 5/27               | 6828 | ExclusiveLock    | t       | f        | 
        (8 rows)
        postgres=# 
