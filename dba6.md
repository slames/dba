#### Создаем пустую БД, настраиваем pgbench. До запуска имеет один файл журнала
          postgres=# select * from pg_ls_waldir();
                     name           |   size   |      modification      
          --------------------------+----------+------------------------
           000000010000000000000001 | 16777216 | 2022-02-15 21:34:56+00
          (1 row)
#### Запускаем тест на 10 минут
          starting vacuum...end.
          progress: 10.0 s, 985.8 tps, lat 8.094 ms stddev 6.028
          progress: 20.0 s, 1024.8 tps, lat 7.804 ms stddev 5.987
          progress: 30.0 s, 1081.5 tps, lat 7.398 ms stddev 5.477
          progress: 40.0 s, 1089.8 tps, lat 7.338 ms stddev 5.418
          progress: 50.0 s, 1104.2 tps, lat 7.245 ms stddev 5.299
          progress: 60.0 s, 1100.6 tps, lat 7.269 ms stddev 5.239
          progress: 70.0 s, 1088.4 tps, lat 7.349 ms stddev 5.272
          progress: 80.0 s, 1088.5 tps, lat 7.349 ms stddev 5.264
          progress: 90.0 s, 1088.2 tps, lat 7.350 ms stddev 5.216
          progress: 100.0 s, 1080.6 tps, lat 7.403 ms stddev 5.199
          progress: 110.0 s, 1090.8 tps, lat 7.334 ms stddev 5.081
          progress: 120.0 s, 1093.5 tps, lat 7.313 ms stddev 5.135
          progress: 130.0 s, 1097.5 tps, lat 7.291 ms stddev 5.176
          progress: 140.0 s, 1094.4 tps, lat 7.307 ms stddev 5.161
          progress: 150.0 s, 1060.7 tps, lat 7.542 ms stddev 5.206
          progress: 160.0 s, 1062.7 tps, lat 7.528 ms stddev 5.197
          progress: 170.0 s, 1059.9 tps, lat 7.546 ms stddev 5.236
          progress: 180.0 s, 1071.6 tps, lat 7.466 ms stddev 5.112
          progress: 190.1 s, 777.0 tps, lat 10.172 ms stddev 18.492
          progress: 200.1 s, 607.4 tps, lat 13.188 ms stddev 25.932
          progress: 210.1 s, 608.2 tps, lat 13.170 ms stddev 25.246
          progress: 220.1 s, 606.0 tps, lat 13.177 ms stddev 25.335
          progress: 230.1 s, 602.3 tps, lat 13.278 ms stddev 25.507
          progress: 240.1 s, 610.9 tps, lat 13.098 ms stddev 25.585
          progress: 250.1 s, 605.9 tps, lat 13.189 ms stddev 25.129
          progress: 260.1 s, 577.3 tps, lat 13.848 ms stddev 26.807
          progress: 270.1 s, 602.0 tps, lat 13.316 ms stddev 25.247
          progress: 280.1 s, 614.0 tps, lat 13.028 ms stddev 24.852
          progress: 290.1 s, 618.2 tps, lat 12.944 ms stddev 24.785
          progress: 300.1 s, 615.8 tps, lat 12.997 ms stddev 24.884
          progress: 310.1 s, 617.5 tps, lat 12.926 ms stddev 24.728
          progress: 320.1 s, 607.8 tps, lat 13.190 ms stddev 25.495
          progress: 330.1 s, 593.8 tps, lat 13.475 ms stddev 24.599
          progress: 340.1 s, 580.8 tps, lat 13.765 ms stddev 26.177
          progress: 350.1 s, 615.0 tps, lat 12.993 ms stddev 24.932
          progress: 360.1 s, 623.7 tps, lat 12.828 ms stddev 24.523
          progress: 370.1 s, 616.6 tps, lat 12.966 ms stddev 25.133
          progress: 380.1 s, 608.8 tps, lat 13.152 ms stddev 25.957
          progress: 390.1 s, 604.7 tps, lat 13.226 ms stddev 25.854
          progress: 400.1 s, 600.8 tps, lat 13.321 ms stddev 26.441
          progress: 410.1 s, 601.3 tps, lat 13.293 ms stddev 26.105
          progress: 420.1 s, 604.5 tps, lat 13.235 ms stddev 26.069
          progress: 430.1 s, 594.8 tps, lat 13.454 ms stddev 26.191
          progress: 440.1 s, 605.4 tps, lat 13.214 ms stddev 26.146
          progress: 450.1 s, 603.2 tps, lat 13.265 ms stddev 26.162
          progress: 460.1 s, 597.9 tps, lat 13.370 ms stddev 26.032
          progress: 470.1 s, 598.8 tps, lat 13.356 ms stddev 26.147
          progress: 480.1 s, 601.4 tps, lat 13.306 ms stddev 26.039
          progress: 490.1 s, 605.1 tps, lat 13.210 ms stddev 25.748
          progress: 500.1 s, 598.3 tps, lat 13.380 ms stddev 26.557
          progress: 510.1 s, 601.1 tps, lat 13.310 ms stddev 25.941
          progress: 520.1 s, 603.3 tps, lat 13.256 ms stddev 25.788
          progress: 530.1 s, 599.3 tps, lat 13.351 ms stddev 25.703
          progress: 540.1 s, 589.7 tps, lat 13.561 ms stddev 25.562
          progress: 550.1 s, 596.9 tps, lat 13.396 ms stddev 25.594
          progress: 560.1 s, 603.8 tps, lat 13.256 ms stddev 25.354
          progress: 570.1 s, 608.2 tps, lat 13.160 ms stddev 25.182
          progress: 580.0 s, 376.6 tps, lat 21.437 ms stddev 20.917
          progress: 590.0 s, 276.8 tps, lat 28.903 ms stddev 14.740
          progress: 600.0 s, 284.4 tps, lat 28.130 ms stddev 15.936
          transaction type: <builtin: TPC-B (sort of)>
          scaling factor: 1
          query mode: simple
          number of clients: 8
          number of threads: 1
          duration: 600 s
          number of transactions actually processed: 440324
          latency average = 10.900 ms
          latency stddev = 19.504 ms
          tps = 733.842866 (including connections establishing)
          tps = 733.846141 (excluding connections establishing)
#### Объем журанала состоялвяет 5 файлов по 16мб. В процессе теста объем журнал немного менялся: 
          postgres=# select * from pg_ls_waldir();
                     name           |   size   |      modification      
          --------------------------+----------+------------------------
           000000010000000000000005 | 16777216 | 2022-02-15 21:38:21+00
           000000010000000000000007 | 16777216 | 2022-02-15 21:37:47+00
           000000010000000000000004 | 16777216 | 2022-02-15 21:37:58+00
           000000010000000000000006 | 16777216 | 2022-02-15 21:38:26+00
          (4 rows)
          postgres=# select * from pg_ls_waldir();
                     name           |   size   |      modification      
          --------------------------+----------+------------------------
           000000010000000000000015 | 16777216 | 2022-02-15 21:43:26+00
           000000010000000000000017 | 16777216 | 2022-02-15 21:42:26+00
           000000010000000000000018 | 16777216 | 2022-02-15 21:42:37+00
           000000010000000000000016 | 16777216 | 2022-02-15 21:41:57+00
           000000010000000000000014 | 16777216 | 2022-02-15 21:42:59+00
          (5 rows)
          postgres=# select * from pg_ls_waldir();
                     name           |   size   |      modification      
          --------------------------+----------+------------------------
           000000010000000000000024 | 16777216 | 2022-02-15 21:47:10+00
           000000010000000000000022 | 16777216 | 2022-02-15 21:46:26+00
           000000010000000000000021 | 16777216 | 2022-02-15 21:45:58+00
           000000010000000000000023 | 16777216 | 2022-02-15 21:46:44+00
           000000010000000000000020 | 16777216 | 2022-02-15 21:47:58+00
          (5 rows)

#### Проверяем лог postgresql на предмет выполнения записи в журнал и понимаем, что не включил логгирование. Устанавливаем параметр log_checkpoints. Смотрим время. +- около 30секунд весь цикл
          2022-02-15 22:00:40.121 UTC [5228] LOG:  checkpoint starting: time
          2022-02-15 22:00:55.102 UTC [5228] LOG:  checkpoint complete: wrote 2013 buffers (12.3%); 0 WAL file(s) added, 0 
          removed, 2 recycled; write=14.952 s, sync=0.006 s, total=14.981 s; sync files=6, longest=0.003 s, average=0.001 s
          ; distance=29215 kB, estimate=29215 kB
          2022-02-15 22:01:10.117 UTC [5228] LOG:  checkpoint starting: time
          2022-02-15 22:01:25.101 UTC [5228] LOG:  checkpoint complete: wrote 2192 buffers (13.4%); 0 WAL file(s) added, 0 
          removed, 2 recycled; write=14.951 s, sync=0.006 s, total=14.984 s; sync files=12, longest=0.003 s, average=0.001 
          s; distance=29554 kB, estimate=29554 kB
          2022-02-15 22:01:40.113 UTC [5228] LOG:  checkpoint starting: time
          2022-02-15 22:01:55.207 UTC [5228] LOG:  checkpoint complete: wrote 2016 buffers (12.3%); 0 WAL file(s) added, 0 
          removed, 1 recycled; write=14.951 s, sync=0.044 s, total=15.095 s; sync files=11, longest=0.023 s, average=0.004 
          s; distance=29448 kB, estimate=29543 kB
          2022-02-15 22:02:10.221 UTC [5228] LOG:  checkpoint starting: time
          2022-02-15 22:02:25.137 UTC [5228] LOG:  checkpoint complete: wrote 1986 buffers (12.1%); 0 WAL file(s) added, 0 
          removed, 1 recycled; write=14.745 s, sync=0.030 s, total=14.916 s; sync files=13, longest=0.030 s, average=0.003 
          s; distance=17718 kB, estimate=28361 kB
          2022-02-15 22:02:40.149 UTC [5228] LOG:  checkpoint starting: time
          2022-02-15 22:02:55.166 UTC [5228] LOG:  checkpoint complete: wrote 1781 buffers (10.9%); 0 WAL file(s) added, 0 
          removed, 1 recycled; write=14.849 s, sync=0.024 s, total=15.018 s; sync files=8, longest=0.024 s, average=0.003 s
          ; distance=17446 kB, estimate=27269 kB
          2022-02-15 22:03:10.181 UTC [5228] LOG:  checkpoint starting: time
          2022-02-15 22:03:25.200 UTC [5228] LOG:  checkpoint complete: wrote 1839 buffers (11.2%); 0 WAL file(s) added, 0 
          removed, 2 recycled; write=14.850 s, sync=0.016 s, total=15.020 s; sync files=12, longest=0.015 s, average=0.002 
          s; distance=17515 kB, estimate=26294 kB
#### Переключаемся в асинхронный режим
          ALTER SYSTEM SET synchronous_commit = off; 
          и перезапуск кластера
#### Прогоняем асинхронный pgbench


