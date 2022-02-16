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
#### Пробуем апдейт из 3 транзакций. 2 заблокированы и тупо висят в консоли
        postgres=# select * from pg_locks;
           locktype    | database | relation | page | tuple | virtualxid | transactionid | classid | objid | objsubid | virtualtransaction | pid  |       mode       | granted | fastpath |           waitstart           
        ---------------+----------+----------+------+-------+------------+---------------+---------+-------+----------+--------------------+------+------------------+---------+----------+-------------------------------
         relation      |    13726 |    16384 |      |       |            |               |         |       |          | 6/360              | 7183 | RowExclusiveLock | t       | t        | 
         virtualxid    |          |          |      |       | 6/360      |               |         |       |          | 6/360              | 7183 | ExclusiveLock    | t       | t        | 
         relation      |    13726 |    16384 |      |       |            |               |         |       |          | 5/30               | 6828 | RowExclusiveLock | t       | t        | 
         virtualxid    |          |          |      |       | 5/30       |               |         |       |          | 5/30               | 6828 | ExclusiveLock    | t       | t        | 
         relation      |    13726 |    16384 |      |       |            |               |         |       |          | 4/26               | 6814 | RowExclusiveLock | t       | t        | 
         virtualxid    |          |          |      |       | 4/26       |               |         |       |          | 4/26               | 6814 | ExclusiveLock    | t       | t        | 
         relation      |    13726 |    12290 |      |       |            |               |         |       |          | 3/15               | 6464 | AccessShareLock  | t       | t        | 
         virtualxid    |          |          |      |       | 3/15       |               |         |       |          | 3/15               | 6464 | ExclusiveLock    | t       | t        | 
         tuple         |    13726 |    16384 |    0 |     5 |            |               |         |       |          | 6/360              | 7183 | ExclusiveLock    | f       | f        | 2022-02-16 12:02:17.040167+00
         transactionid |          |          |      |       |            |           739 |         |       |          | 4/26               | 6814 | ShareLock        | f       | f        | 2022-02-16 12:02:00.035264+00
         transactionid |          |          |      |       |            |           740 |         |       |          | 4/26               | 6814 | ExclusiveLock    | t       | f        | 
         transactionid |          |          |      |       |            |           741 |         |       |          | 6/360              | 7183 | ExclusiveLock    | t       | f        | 
         transactionid |          |          |      |       |            |           739 |         |       |          | 5/30               | 6828 | ExclusiveLock    | t       | f        | 
         tuple         |    13726 |    16384 |    0 |     5 |            |               |         |       |          | 4/26               | 6814 | ExclusiveLock    | t       | f        | 
        (14 rows)
#### Первая транзакция захватила блокировку (739) две другие ждут снятия блокировки. Также видны 3 попытки получить блокировку конкретной строчки на изменение
