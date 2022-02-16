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
         Интересны следующие: 
         ExclusiveLock - Блокируют на изменение другими транзакциями, остается только функция чтения (один писатель - много читателей)
         RowExclusiveLock - Заблокированна строка, идет операция записи или изменения. Все наши 3 апдейта поставили эти блокировки
         ShareLock - защита от одновременного изменения. Этот лок поставил первая транзакция  
 
#### Расследуем в момент. Блокировка висит. Открывает лог и все довольно понятно
        2022-02-16 12:02:00.235 UTC [6814] postgres@postgres LOG:  process 6814 still waiting for ShareLock on transaction 739 after 200.208 ms
        2022-02-16 12:02:00.235 UTC [6814] postgres@postgres DETAIL:  Process holding the lock: 6828. Wait queue: 6814.
        2022-02-16 12:02:00.235 UTC [6814] postgres@postgres CONTEXT:  while updating tuple (0,5) in relation "test1"
        2022-02-16 12:02:00.235 UTC [6814] postgres@postgres STATEMENT:  update test1 set name = 'updatecheck2' where id = 1;
        2022-02-16 12:02:17.240 UTC [7183] postgres@postgres LOG:  process 7183 still waiting for ExclusiveLock on tuple (0,5) of relation 16384 of database 13726 after 200.189 ms
        2022-02-16 12:02:17.240 UTC [7183] postgres@postgres DETAIL:  Process holding the lock: 6814. Wait queue: 7183.
        2022-02-16 12:02:17.240 UTC [7183] postgres@postgres STATEMENT:  update test1 set name = 'updatecheck3' where id = 1;
        
#### После коммита первой транзакции исполнилась следующая, ставшая в очередь
#### Проверяем возможность блокировки UPDATE без условий. Кажется логичным, что блокировка будет. UPDATE без условия будет еще более жестким. Например, заменим поле name во всех записях. Убедимся. 
        postgres=# BEGIN;
        BEGIN
        postgres=*# update test1 set name = 'updatecheck2';
#### Блокировка происходит
        postgres=# select * from pg_locks;
           locktype    | database | relation | page | tuple | virtualxid | transactionid | classid | objid | objsubid | virtualtransaction | pid  |       mode       | granted | fastpath |           waitstart           
        ---------------+----------+----------+------+-------+------------+---------------+---------+-------+----------+--------------------+------+------------------+---------+----------+-------------------------------
         relation      |    13726 |    16384 |      |       |            |               |         |       |          | 5/31               | 6828 | RowExclusiveLock | t       | t        | 
         virtualxid    |          |          |      |       | 5/31       |               |         |       |          | 5/31               | 6828 | ExclusiveLock    | t       | t        | 
         relation      |    13726 |    16384 |      |       |            |               |         |       |          | 4/27               | 6814 | RowExclusiveLock | t       | t        | 
         virtualxid    |          |          |      |       | 4/27       |               |         |       |          | 4/27               | 6814 | ExclusiveLock    | t       | t        | 
         relation      |    13726 |    12290 |      |       |            |               |         |       |          | 3/43               | 7263 | AccessShareLock  | t       | t        | 
         virtualxid    |          |          |      |       | 3/43       |               |         |       |          | 3/43               | 7263 | ExclusiveLock    | t       | t        | 
         tuple         |    13726 |    16384 |    0 |     3 |            |               |         |       |          | 4/27               | 6814 | ExclusiveLock    | t       | f        | 
         transactionid |          |          |      |       |            |           743 |         |       |          | 4/27               | 6814 | ExclusiveLock    | t       | f        | 
         transactionid |          |          |      |       |            |           742 |         |       |          | 5/31               | 6828 | ExclusiveLock    | t       | f        | 
         transactionid |          |          |      |       |            |           742 |         |       |          | 4/27               | 6814 | ShareLock        | f       | f        | 2022-02-16 12:16:21.204537+00

        В логах: 
        2022-02-16 12:16:21.404 UTC [6814] postgres@postgres LOG:  process 6814 still waiting for ShareLock on transaction 742 after 200.140 ms
        2022-02-16 12:16:21.404 UTC [6814] postgres@postgres DETAIL:  Process holding the lock: 6828. Wait queue: 6814.
        2022-02-16 12:16:21.404 UTC [6814] postgres@postgres CONTEXT:  while updating tuple (0,3) in relation "test1"
        2022-02-16 12:16:21.404 UTC [6814] postgres@postgres STATEMENT:  update test1 set name = 'updatecheck2';


