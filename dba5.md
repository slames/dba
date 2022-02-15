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
 #### Стартовый прогон. Падение скорости дважды без вакума
	 starting vacuum...end.
	progress: 10.0 s, 1096.9 tps, lat 7.273 ms stddev 4.380
	progress: 20.0 s, 1127.5 tps, lat 7.095 ms stddev 4.333
	progress: 30.0 s, 1120.8 tps, lat 7.136 ms stddev 4.484
	progress: 40.0 s, 1108.5 tps, lat 7.215 ms stddev 4.404
	progress: 50.0 s, 1060.9 tps, lat 7.542 ms stddev 4.557
	progress: 60.0 s, 1062.4 tps, lat 7.530 ms stddev 4.869
	progress: 70.0 s, 1106.2 tps, lat 7.231 ms stddev 4.353
	progress: 80.0 s, 1097.2 tps, lat 7.290 ms stddev 4.273
	progress: 90.0 s, 1113.8 tps, lat 7.182 ms stddev 4.315
	progress: 100.0 s, 1112.5 tps, lat 7.189 ms stddev 5.000
	progress: 110.0 s, 1141.0 tps, lat 7.012 ms stddev 5.327
	progress: 120.0 s, 1117.7 tps, lat 7.156 ms stddev 5.636
	progress: 130.0 s, 1129.7 tps, lat 7.081 ms stddev 5.585
	progress: 140.0 s, 1164.9 tps, lat 6.868 ms stddev 5.343
	progress: 150.0 s, 1127.0 tps, lat 7.093 ms stddev 5.608
	progress: 160.0 s, 1122.1 tps, lat 7.132 ms stddev 5.647
	progress: 170.0 s, 1148.1 tps, lat 6.967 ms stddev 5.500
	progress: 180.0 s, 1132.3 tps, lat 7.065 ms stddev 5.500
	progress: 190.0 s, 1148.1 tps, lat 6.967 ms stddev 5.363
	progress: 200.0 s, 1128.1 tps, lat 7.091 ms stddev 5.702
	progress: 210.0 s, 1135.0 tps, lat 7.046 ms stddev 5.540
	progress: 220.0 s, 648.0 tps, lat 12.280 ms stddev 24.027
	progress: 230.0 s, 643.9 tps, lat 12.456 ms stddev 25.152
	progress: 240.0 s, 631.5 tps, lat 12.652 ms stddev 25.139
	progress: 250.0 s, 626.2 tps, lat 12.774 ms stddev 25.686
	progress: 260.0 s, 638.7 tps, lat 12.543 ms stddev 24.854
	progress: 270.0 s, 647.1 tps, lat 12.328 ms stddev 24.952
	progress: 280.0 s, 646.0 tps, lat 12.369 ms stddev 24.855
	progress: 290.0 s, 605.2 tps, lat 13.253 ms stddev 26.388
	progress: 300.0 s, 634.4 tps, lat 12.610 ms stddev 24.877
	Стандартный запуск: 
	autovacuum_vacuum_scale_factor = 0.2
	autovacuum_vacuum_scale_factor = 0.2
	autovacuum_vacuum_scale_factor = 0.2
	autovacuum_vacuum_scale_factor = 0.2
	autovacuum = on    
	progress: 310.0 s, 626.1 tps, lat 12.773 ms stddev 25.365
	progress: 320.0 s, 653.0 tps, lat 12.234 ms stddev 24.470
	progress: 330.0 s, 643.1 tps, lat 12.419 ms stddev 24.329
	progress: 340.0 s, 641.9 tps, lat 12.483 ms stddev 24.349
	progress: 350.0 s, 648.5 tps, lat 12.336 ms stddev 24.452
	progress: 360.0 s, 619.0 tps, lat 12.923 ms stddev 24.570
	progress: 370.0 s, 611.9 tps, lat 13.088 ms stddev 25.208
	progress: 380.0 s, 650.0 tps, lat 12.288 ms stddev 24.352
	progress: 390.0 s, 649.4 tps, lat 12.320 ms stddev 24.676
	progress: 400.0 s, 641.8 tps, lat 12.465 ms stddev 24.257
	progress: 410.0 s, 653.3 tps, lat 12.244 ms stddev 24.499
	progress: 420.0 s, 637.8 tps, lat 12.560 ms stddev 24.728
	progress: 430.0 s, 640.2 tps, lat 12.497 ms stddev 24.524
	progress: 440.0 s, 648.3 tps, lat 12.322 ms stddev 24.778
	progress: 450.0 s, 649.6 tps, lat 12.330 ms stddev 24.430
	progress: 460.0 s, 644.2 tps, lat 12.365 ms stddev 24.336
	progress: 470.0 s, 639.3 tps, lat 12.582 ms stddev 24.914
	progress: 480.0 s, 632.4 tps, lat 12.613 ms stddev 25.231
	progress: 490.0 s, 627.4 tps, lat 12.788 ms stddev 25.790
	progress: 500.0 s, 468.5 tps, lat 17.048 ms stddev 23.338
	progress: 510.0 s, 297.3 tps, lat 26.907 ms stddev 14.672
	progress: 520.0 s, 296.7 tps, lat 26.977 ms stddev 13.760
	progress: 530.0 s, 297.2 tps, lat 26.916 ms stddev 15.315
	progress: 540.0 s, 297.1 tps, lat 26.923 ms stddev 14.089
	progress: 550.0 s, 296.8 tps, lat 26.946 ms stddev 14.376
	progress: 560.0 s, 293.6 tps, lat 27.247 ms stddev 15.441
	progress: 570.0 s, 297.4 tps, lat 26.900 ms stddev 14.906
	progress: 580.0 s, 297.0 tps, lat 26.943 ms stddev 13.834
	progress: 590.0 s, 293.8 tps, lat 27.228 ms stddev 14.821
	progress: 600.0 s, 296.9 tps, lat 26.934 ms stddev 14.317
	transaction type: <builtin: TPC-B (sort of)>
	scaling factor: 1
	query mode: simple
	number of clients: 8
	number of threads: 1
	duration: 600 s
 
 
#### Некуй конфиг с параметрами от percona по статье про vacuum
	starting vacuum...end.
	progress: 10.0 s, 1107.1 tps, lat 7.206 ms stddev 5.545
	progress: 20.0 s, 1117.7 tps, lat 7.156 ms stddev 5.392
	progress: 30.0 s, 1138.1 tps, lat 7.028 ms stddev 5.325
	progress: 40.0 s, 1127.3 tps, lat 7.097 ms stddev 5.399
	progress: 50.0 s, 1104.3 tps, lat 7.243 ms stddev 5.431
	progress: 60.0 s, 1099.0 tps, lat 7.278 ms stddev 5.327
	progress: 70.0 s, 1138.5 tps, lat 7.023 ms stddev 5.232
	progress: 80.0 s, 1131.9 tps, lat 7.066 ms stddev 5.250
	progress: 90.0 s, 1135.3 tps, lat 7.047 ms stddev 5.274
	progress: 100.0 s, 1128.6 tps, lat 7.091 ms stddev 5.162
	progress: 110.0 s, 1140.4 tps, lat 7.012 ms stddev 5.017
	progress: 120.0 s, 1127.1 tps, lat 7.099 ms stddev 5.205
	progress: 130.0 s, 1152.3 tps, lat 6.942 ms stddev 4.933
	progress: 140.0 s, 1146.2 tps, lat 6.976 ms stddev 5.060
	progress: 150.0 s, 1125.9 tps, lat 7.108 ms stddev 5.086
	progress: 160.0 s, 1148.9 tps, lat 6.960 ms stddev 4.857
	progress: 170.0 s, 1101.0 tps, lat 7.265 ms stddev 5.216
	progress: 180.0 s, 1056.9 tps, lat 7.570 ms stddev 5.448
	progress: 190.0 s, 1102.0 tps, lat 7.259 ms stddev 4.978
	progress: 200.0 s, 669.3 tps, lat 11.834 ms stddev 23.210
	progress: 210.0 s, 638.2 tps, lat 12.529 ms stddev 24.181
	progress: 220.0 s, 641.6 tps, lat 12.464 ms stddev 24.117
	progress: 230.0 s, 626.9 tps, lat 12.756 ms stddev 24.577
	progress: 240.0 s, 628.7 tps, lat 12.721 ms stddev 24.968
	progress: 250.0 s, 644.6 tps, lat 12.411 ms stddev 23.756
	progress: 260.0 s, 646.0 tps, lat 12.387 ms stddev 24.180
	progress: 270.0 s, 636.0 tps, lat 12.578 ms stddev 24.060
	progress: 280.0 s, 644.0 tps, lat 12.425 ms stddev 23.956
	progress: 290.0 s, 643.8 tps, lat 12.432 ms stddev 24.296
	progress: 300.0 s, 632.6 tps, lat 12.645 ms stddev 24.846
	autovacuum = off
	progress: 310.0 s, 657.5 tps, lat 12.172 ms stddev 24.294
	progress: 320.0 s, 644.0 tps, lat 12.410 ms stddev 24.051
	progress: 330.0 s, 647.1 tps, lat 12.355 ms stddev 24.190
	progress: 340.0 s, 644.9 tps, lat 12.410 ms stddev 24.289
	progress: 350.0 s, 633.5 tps, lat 12.603 ms stddev 25.361
	progress: 360.0 s, 617.7 tps, lat 12.951 ms stddev 26.063
	progress: 370.0 s, 630.4 tps, lat 12.711 ms stddev 24.728
	progress: 380.0 s, 619.9 tps, lat 12.875 ms stddev 25.703
	progress: 390.0 s, 626.0 tps, lat 12.789 ms stddev 25.374
	progress: 400.0 s, 636.9 tps, lat 12.561 ms stddev 25.318
	progress: 410.0 s, 621.1 tps, lat 12.878 ms stddev 26.033
	progress: 420.0 s, 613.7 tps, lat 13.035 ms stddev 25.341
	progress: 430.0 s, 624.0 tps, lat 12.824 ms stddev 25.418
	progress: 440.0 s, 628.7 tps, lat 12.716 ms stddev 25.152
	progress: 450.0 s, 628.0 tps, lat 12.740 ms stddev 25.657
	progress: 460.0 s, 621.6 tps, lat 12.870 ms stddev 25.006
	progress: 470.0 s, 616.5 tps, lat 12.978 ms stddev 25.567
	progress: 480.0 s, 584.0 tps, lat 13.708 ms stddev 26.487
	progress: 490.0 s, 596.5 tps, lat 13.393 ms stddev 25.024
	progress: 500.0 s, 625.8 tps, lat 12.780 ms stddev 25.056
	progress: 510.0 s, 620.2 tps, lat 12.909 ms stddev 24.982
	progress: 520.0 s, 623.0 tps, lat 12.838 ms stddev 24.942
	progress: 530.0 s, 615.2 tps, lat 13.005 ms stddev 25.427
	progress: 540.0 s, 313.2 tps, lat 25.772 ms stddev 15.547
	progress: 550.0 s, 296.8 tps, lat 26.935 ms stddev 12.928
	progress: 560.0 s, 292.6 tps, lat 27.351 ms stddev 13.941
	progress: 570.0 s, 299.6 tps, lat 26.720 ms stddev 12.835
	progress: 580.0 s, 299.6 tps, lat 26.698 ms stddev 12.684
	progress: 590.0 s, 292.6 tps, lat 27.338 ms stddev 14.229
	progress: 600.0 s, 295.7 tps, lat 27.061 ms stddev 14.291
	transaction type: <builtin: TPC-B (sort of)>
	scaling factor: 1
	query mode: simple
	number of clients: 8
	number of threads: 1
	duration: 600 s

#### От прогонов наблюдается снижение производительности. Cloud явно определяет нагрузку и урезает мощности. Особенно хорошо видно на втором прогоне с выключенным вакумом. Он был сразу после первого. 
#### Выключен автовакум 1 запуск
	starting vacuum...end.
	progress: 10.0 s, 1174.4 tps, lat 6.795 ms stddev 4.068
	progress: 20.0 s, 1130.2 tps, lat 7.077 ms stddev 4.279
	progress: 30.0 s, 1123.7 tps, lat 7.117 ms stddev 4.379
	progress: 40.0 s, 1137.6 tps, lat 7.033 ms stddev 4.154
	progress: 50.0 s, 1145.0 tps, lat 6.986 ms stddev 4.153
	progress: 60.0 s, 1160.9 tps, lat 6.891 ms stddev 3.994
	progress: 70.0 s, 1163.0 tps, lat 6.877 ms stddev 3.965
	progress: 80.0 s, 1146.5 tps, lat 6.978 ms stddev 3.974
	progress: 90.0 s, 1140.8 tps, lat 7.009 ms stddev 4.025
	progress: 100.0 s, 1161.6 tps, lat 6.888 ms stddev 3.955
	progress: 110.0 s, 1189.9 tps, lat 6.723 ms stddev 5.070
	progress: 120.0 s, 1172.4 tps, lat 6.822 ms stddev 5.312
	progress: 130.0 s, 1157.4 tps, lat 6.911 ms stddev 5.357
	progress: 140.0 s, 1184.9 tps, lat 6.750 ms stddev 5.175
	progress: 150.0 s, 1171.3 tps, lat 6.832 ms stddev 5.220
	progress: 160.0 s, 1166.7 tps, lat 6.853 ms stddev 5.295
	progress: 170.0 s, 1167.4 tps, lat 6.852 ms stddev 5.368
	progress: 180.0 s, 1152.2 tps, lat 6.943 ms stddev 5.288
	progress: 190.0 s, 1151.4 tps, lat 6.947 ms stddev 5.245
	progress: 200.0 s, 845.8 tps, lat 9.461 ms stddev 18.115
	progress: 210.0 s, 658.8 tps, lat 12.141 ms stddev 25.240
	progress: 220.0 s, 655.0 tps, lat 12.209 ms stddev 25.228
	progress: 230.0 s, 656.8 tps, lat 12.180 ms stddev 24.841
	progress: 240.0 s, 642.3 tps, lat 12.456 ms stddev 24.841
	progress: 250.0 s, 629.8 tps, lat 12.697 ms stddev 24.740
	progress: 260.0 s, 645.3 tps, lat 12.399 ms stddev 24.470
	progress: 270.0 s, 650.3 tps, lat 12.299 ms stddev 24.547
	progress: 280.0 s, 654.5 tps, lat 12.210 ms stddev 24.147
	progress: 290.0 s, 654.7 tps, lat 12.220 ms stddev 24.105
	progress: 300.0 s, 655.0 tps, lat 12.209 ms stddev 24.100
	progress: 310.0 s, 657.0 tps, lat 12.180 ms stddev 24.045
	progress: 320.0 s, 659.3 tps, lat 12.133 ms stddev 24.116
	progress: 330.0 s, 661.2 tps, lat 12.099 ms stddev 23.829
	progress: 340.0 s, 659.9 tps, lat 12.124 ms stddev 23.953
	progress: 350.0 s, 660.8 tps, lat 12.104 ms stddev 23.708
	progress: 360.0 s, 661.1 tps, lat 12.101 ms stddev 23.803
	progress: 370.0 s, 659.0 tps, lat 12.138 ms stddev 24.200
	progress: 380.0 s, 659.9 tps, lat 12.123 ms stddev 23.785
	progress: 390.0 s, 620.0 tps, lat 12.902 ms stddev 25.447
	progress: 400.0 s, 660.3 tps, lat 12.114 ms stddev 23.819
	progress: 410.0 s, 659.5 tps, lat 12.129 ms stddev 23.866
	progress: 420.0 s, 670.8 tps, lat 11.927 ms stddev 23.526
	progress: 430.0 s, 667.4 tps, lat 11.986 ms stddev 23.806
	progress: 440.0 s, 654.4 tps, lat 12.223 ms stddev 24.183
	progress: 450.0 s, 662.3 tps, lat 12.077 ms stddev 23.940
	progress: 460.0 s, 669.4 tps, lat 11.950 ms stddev 24.212
	progress: 470.0 s, 666.3 tps, lat 12.007 ms stddev 24.569
	progress: 480.0 s, 668.4 tps, lat 11.966 ms stddev 24.420
	progress: 490.0 s, 371.6 tps, lat 21.507 ms stddev 19.563
	progress: 500.0 s, 296.9 tps, lat 26.943 ms stddev 15.120
	progress: 510.0 s, 297.6 tps, lat 26.887 ms stddev 14.809
	progress: 520.0 s, 292.4 tps, lat 27.345 ms stddev 15.036
	progress: 530.0 s, 299.5 tps, lat 26.723 ms stddev 15.094
	progress: 540.0 s, 298.0 tps, lat 26.831 ms stddev 14.485
	progress: 550.0 s, 297.7 tps, lat 26.881 ms stddev 15.476
	progress: 560.0 s, 294.2 tps, lat 27.203 ms stddev 14.124
	progress: 570.0 s, 296.2 tps, lat 26.994 ms stddev 14.254
	progress: 580.0 s, 298.2 tps, lat 26.835 ms stddev 14.438
	progress: 590.0 s, 297.4 tps, lat 26.905 ms stddev 14.965
	progress: 600.0 s, 298.1 tps, lat 26.829 ms stddev 14.958
	
#### 2 запуск c выключенным avtovacuum
	progress: 10.0 s, 1165.3 tps, lat 6.847 ms stddev 4.813
	progress: 20.0 s, 1168.6 tps, lat 6.845 ms stddev 4.821
	progress: 30.0 s, 1197.6 tps, lat 6.680 ms stddev 4.552
	progress: 40.0 s, 1150.3 tps, lat 6.954 ms stddev 4.881
	progress: 50.0 s, 1150.3 tps, lat 6.954 ms stddev 4.903
	progress: 60.0 s, 737.6 tps, lat 10.832 ms stddev 11.278
	progress: 70.0 s, 299.5 tps, lat 26.722 ms stddev 13.771
	progress: 80.0 s, 298.3 tps, lat 26.817 ms stddev 13.751
	progress: 90.0 s, 296.9 tps, lat 26.933 ms stddev 13.117
	progress: 100.0 s, 294.9 tps, lat 27.129 ms stddev 13.547
	progress: 110.0 s, 297.1 tps, lat 26.926 ms stddev 13.388
	progress: 120.0 s, 297.4 tps, lat 26.896 ms stddev 13.080
	progress: 130.0 s, 298.2 tps, lat 26.840 ms stddev 12.493
	progress: 140.0 s, 297.4 tps, lat 26.894 ms stddev 13.135
	progress: 150.0 s, 297.7 tps, lat 26.869 ms stddev 12.301
	progress: 160.0 s, 298.1 tps, lat 26.832 ms stddev 13.108
	progress: 170.0 s, 298.2 tps, lat 26.832 ms stddev 13.319
	progress: 180.0 s, 298.0 tps, lat 26.837 ms stddev 13.755
	progress: 190.0 s, 297.5 tps, lat 26.887 ms stddev 13.222
	progress: 200.0 s, 298.0 tps, lat 26.857 ms stddev 13.647
	progress: 210.0 s, 291.4 tps, lat 27.451 ms stddev 17.793
	progress: 220.0 s, 296.7 tps, lat 26.966 ms stddev 12.952
	progress: 230.0 s, 297.8 tps, lat 26.850 ms stddev 13.327
	progress: 240.0 s, 298.1 tps, lat 26.844 ms stddev 12.703
	progress: 250.0 s, 298.1 tps, lat 26.823 ms stddev 12.849
	progress: 260.0 s, 297.5 tps, lat 26.897 ms stddev 12.468
	progress: 270.0 s, 297.6 tps, lat 26.884 ms stddev 12.606
	progress: 280.0 s, 298.0 tps, lat 26.848 ms stddev 12.927
	progress: 290.0 s, 298.0 tps, lat 26.847 ms stddev 12.529
	progress: 300.0 s, 296.7 tps, lat 26.961 ms stddev 12.694
	progress: 310.0 s, 297.3 tps, lat 26.904 ms stddev 13.508
	progress: 320.0 s, 297.8 tps, lat 26.864 ms stddev 14.224
	progress: 330.0 s, 297.3 tps, lat 26.877 ms stddev 13.078
	progress: 340.0 s, 297.9 tps, lat 26.887 ms stddev 13.432
	progress: 350.0 s, 292.1 tps, lat 27.392 ms stddev 15.345
	progress: 360.0 s, 299.6 tps, lat 26.684 ms stddev 12.965
	progress: 370.0 s, 298.0 tps, lat 26.860 ms stddev 13.735
	progress: 380.0 s, 299.6 tps, lat 26.698 ms stddev 12.733
	progress: 390.0 s, 296.7 tps, lat 26.959 ms stddev 12.846
	progress: 400.0 s, 294.3 tps, lat 27.182 ms stddev 13.901
	progress: 410.0 s, 297.7 tps, lat 26.875 ms stddev 14.425
	progress: 420.0 s, 297.8 tps, lat 26.866 ms stddev 14.852
	progress: 430.0 s, 298.1 tps, lat 26.831 ms stddev 13.747
	progress: 440.0 s, 297.0 tps, lat 26.913 ms stddev 12.966
	progress: 450.0 s, 294.4 tps, lat 27.198 ms stddev 15.366
	progress: 460.0 s, 298.1 tps, lat 26.827 ms stddev 14.103
	progress: 470.0 s, 297.7 tps, lat 26.885 ms stddev 14.034
	progress: 480.0 s, 298.0 tps, lat 26.840 ms stddev 15.712
	progress: 490.0 s, 298.0 tps, lat 26.836 ms stddev 14.710
	progress: 500.0 s, 298.2 tps, lat 26.830 ms stddev 14.429
	progress: 510.0 s, 297.8 tps, lat 26.858 ms stddev 14.623
	progress: 520.0 s, 297.4 tps, lat 26.865 ms stddev 14.907
	progress: 530.0 s, 297.8 tps, lat 26.916 ms stddev 14.364
	progress: 540.0 s, 298.0 tps, lat 26.839 ms stddev 14.825
	progress: 550.0 s, 298.0 tps, lat 26.839 ms stddev 14.926
	progress: 560.0 s, 298.1 tps, lat 26.848 ms stddev 14.439
	progress: 570.0 s, 297.3 tps, lat 26.906 ms stddev 13.436
	progress: 580.0 s, 296.8 tps, lat 26.936 ms stddev 14.655
	progress: 590.0 s, 297.9 tps, lat 26.864 ms stddev 13.403
	progress: 600.0 s, 298.0 tps, lat 26.845 ms stddev 13.111

#### Пробуем оптимизации. Фактически влияния нет. Основной эффект от паузы между запусками, чтобы облако перестало резать ресурс
#### Пример конфига 1
	autovacuum = on
	track_counts = on
	autovacuum_vacuum_scale_factor = 0.01
	autovacuum_vacuum_threshold = 5000
	autovacuum_analyze_scale_factor = 0.01
	autovacuum_analyze_threshold = 5000
	autovacuum_vacuum_cost_limit = -1
	autovacuum_vacuum_cost_delay = 20ms
	vacuum_cost_page_hit = 1
	vacuum_cost_page_miss = 10
	vacuum_cost_page_dirty = 20
	autovacuum_vacuum_insert_scale_factor = 0.2
	autovacuum_vacuum_insert_threshold =1000
	autovacuum_work_mem = -1
	log_autovacuum_min_duration = -1
	autovacuum_freeze_max_age = 200000000

# Пример конфига 2

	autovacuum = on
	track_counts = on
	autovacuum_vacuum_scale_factor = 0.01
	autovacuum_vacuum_threshold = 5000
	autovacuum_analyze_scale_factor = 0.01
	autovacuum_analyze_threshold = 5000
	autovacuum_vacuum_cost_limit = -1
	autovacuum_vacuum_cost_delay = 20ms
	vacuum_cost_page_hit = 1
	vacuum_cost_page_miss = 10
	vacuum_cost_page_dirty = 20
	autovacuum_vacuum_insert_scale_factor = 0.2
	autovacuum_vacuum_insert_threshold =1000
	autovacuum_work_mem = -1
	log_autovacuum_min_duration = -1
	autovacuum_freeze_max_age = 200000000
	autovacuum_naptime = 1
# результат прогона последнего конфига 
	postgres@dba5:/etc/postgresql/13/main/conf.d$ pgbench -c8 -P 10 -T 600 -U postgres postgres
	starting vacuum...end.
	progress: 10.0 s, 1091.5 tps, lat 7.309 ms stddev 5.740
	progress: 20.0 s, 1119.4 tps, lat 7.146 ms stddev 5.567
	progress: 30.0 s, 1123.6 tps, lat 7.120 ms stddev 5.745
	progress: 40.0 s, 1126.0 tps, lat 7.102 ms stddev 5.649
	progress: 50.0 s, 1122.2 tps, lat 7.129 ms stddev 5.614
	progress: 60.0 s, 1108.1 tps, lat 7.219 ms stddev 5.575
	progress: 70.0 s, 1118.0 tps, lat 7.155 ms stddev 5.507
	progress: 80.0 s, 1103.2 tps, lat 7.248 ms stddev 5.436
	progress: 90.0 s, 1109.6 tps, lat 7.205 ms stddev 5.553
	progress: 100.0 s, 1065.3 tps, lat 7.514 ms stddev 5.487
	progress: 110.0 s, 1118.8 tps, lat 7.151 ms stddev 5.432
	progress: 120.0 s, 1125.1 tps, lat 7.108 ms stddev 5.227
	progress: 130.0 s, 1119.8 tps, lat 7.144 ms stddev 5.364
	progress: 140.0 s, 1099.0 tps, lat 7.278 ms stddev 5.449
	progress: 150.0 s, 1100.9 tps, lat 7.265 ms stddev 5.448
	progress: 160.0 s, 1114.7 tps, lat 7.176 ms stddev 5.086
	progress: 170.0 s, 1066.8 tps, lat 7.499 ms stddev 5.680
	progress: 180.0 s, 1055.1 tps, lat 7.580 ms stddev 5.688
	progress: 190.0 s, 1098.3 tps, lat 7.284 ms stddev 5.186
	progress: 200.0 s, 967.9 tps, lat 8.264 ms stddev 12.042
	progress: 210.0 s, 651.5 tps, lat 12.278 ms stddev 23.663
	progress: 220.0 s, 634.5 tps, lat 12.608 ms stddev 24.715
	progress: 230.0 s, 632.4 tps, lat 12.650 ms stddev 24.969
	progress: 240.0 s, 637.6 tps, lat 12.546 ms stddev 25.106
	progress: 250.0 s, 652.1 tps, lat 12.262 ms stddev 23.767
	progress: 260.0 s, 630.0 tps, lat 12.704 ms stddev 24.454
	progress: 270.0 s, 625.2 tps, lat 12.791 ms stddev 24.639
	progress: 280.0 s, 618.4 tps, lat 12.935 ms stddev 24.687
	progress: 290.0 s, 647.1 tps, lat 12.366 ms stddev 23.815
	progress: 300.0 s, 625.8 tps, lat 12.780 ms stddev 25.664
	progress: 310.0 s, 622.3 tps, lat 12.857 ms stddev 25.525
	progress: 320.0 s, 634.7 tps, lat 12.599 ms stddev 24.606
	progress: 330.0 s, 623.5 tps, lat 12.829 ms stddev 26.081
	progress: 340.0 s, 585.2 tps, lat 13.674 ms stddev 28.148
	progress: 350.0 s, 630.8 tps, lat 12.681 ms stddev 25.244
	progress: 360.0 s, 624.1 tps, lat 12.817 ms stddev 25.922
	progress: 370.0 s, 620.6 tps, lat 12.891 ms stddev 25.933
	progress: 380.0 s, 630.8 tps, lat 12.678 ms stddev 25.522
	progress: 390.0 s, 611.6 tps, lat 13.077 ms stddev 26.409
	progress: 400.0 s, 619.0 tps, lat 12.910 ms stddev 25.735
	progress: 410.0 s, 638.8 tps, lat 12.519 ms stddev 24.881
	progress: 420.0 s, 615.1 tps, lat 12.994 ms stddev 26.055
	progress: 430.0 s, 616.0 tps, lat 12.998 ms stddev 25.848
	progress: 440.0 s, 642.4 tps, lat 12.453 ms stddev 24.873
	progress: 450.0 s, 620.6 tps, lat 12.890 ms stddev 25.906
	progress: 460.0 s, 617.7 tps, lat 12.948 ms stddev 26.024
	progress: 470.0 s, 632.2 tps, lat 12.658 ms stddev 25.233
	progress: 480.0 s, 608.8 tps, lat 13.136 ms stddev 25.712
	progress: 490.0 s, 603.0 tps, lat 13.260 ms stddev 25.529
	progress: 500.0 s, 636.2 tps, lat 12.583 ms stddev 25.208
	progress: 510.0 s, 618.5 tps, lat 12.931 ms stddev 25.886
	progress: 520.0 s, 637.7 tps, lat 12.544 ms stddev 24.540
	progress: 530.0 s, 624.4 tps, lat 12.811 ms stddev 25.478
	progress: 540.0 s, 366.8 tps, lat 21.795 ms stddev 21.496
	progress: 550.0 s, 297.4 tps, lat 26.886 ms stddev 17.118
	progress: 560.0 s, 299.4 tps, lat 26.691 ms stddev 15.719
	progress: 570.0 s, 297.4 tps, lat 26.915 ms stddev 15.390
	progress: 580.0 s, 292.7 tps, lat 27.354 ms stddev 16.206
	progress: 590.0 s, 295.4 tps, lat 27.071 ms stddev 16.731
	progress: 600.0 s, 296.8 tps, lat 26.936 ms stddev 16.472
	transaction type: <builtin: TPC-B (sort of)>
	scaling factor: 1
	query mode: simple
	number of clients: 8
	number of threads: 1
	duration: 600 s
	number of transactions actually processed: 447686
	latency average = 10.720 ms
	latency stddev = 18.629 ms
	tps = 746.113267 (including connections establishing)
	tps = 746.116663 (excluding connections establishing)
#### Итоговый график
![vacuum](https://user-images.githubusercontent.com/95148132/154134434-da63fda3-96df-4a81-b649-959c6162b7b8.png)

#### Вывод. Оптимизация не заметна. Скорее поведение cloud видно. При росте нагрузки система душит виртуалку и все остальное остается мелочью на фоне. 
