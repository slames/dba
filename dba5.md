#### Создаем БД c 13 postgresql на ubuntu
#### В папке conf.d создаем конфиг с рекомендуемыми параметрами
        max_connections = 40
        shared_buffers = 1GB
        effective_cache_size = 3GB
        maintenance_work_mem = 512MB
        checkpoint_completion_target = 0.9
         wal_buffers = 16MB
        default_statistics_target = 500
        random_page_cost = 4
        effective_io_concurrency = 2
        work_mem = 6553kB
        min_wal_size = 4GB
        max_wal_size = 16GB
####  Делаем базовую настройку и прогон 
        postgres@dba5:/etc/postgresql/13/main/conf.d$ pgbench -i postgres
        dropping old tables...
        NOTICE:  table "pgbench_accounts" does not exist, skipping
        NOTICE:  table "pgbench_branches" does not exist, skipping
        NOTICE:  table "pgbench_history" does not exist, skipping
        NOTICE:  table "pgbench_tellers" does not exist, skipping
        creating tables...
        generating data (client-side)...
        100000 of 100000 tuples (100%) done (elapsed 0.10 s, remaining 0.00 s)
        vacuuming...
        creating primary keys...
        done in 0.44 s (drop tables 0.00 s, create tables 0.01 s, client-side generate 0.29 s, vacuum 0.08 s, primary keys 0.07 s).
 #### Стартовый прогон
        progress: 600.0 s, 296.9 tps, lat 26.934 ms stddev 14.317
        transaction type: <builtin: TPC-B (sort of)>
        scaling factor: 1
        query mode: simple
        number of clients: 8
        number of threads: 1
        duration: 600 s
        number of transactions actually processed: 448120
        latency average = 10.710 ms
        latency stddev = 17.467 ms
        tps = 746.834296 (including connections establishing)
        tps = 746.837514 (excluding connections establishing)
 #### 
