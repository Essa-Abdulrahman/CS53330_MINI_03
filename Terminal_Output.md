Windows PowerShell 5.1
Copyright (C) Microsoft Corporation. All rights reserved.

PS C:\CS5200_Practicum2> docker compose up -d
[+] up 20/20
 ✔ Image postgres:16                 Pulled                                                                          23.7s
 ✔ Volume cs5200_practicum2_pgdata   Created                                                                          0.0s
 ✔ Network cs5200_practicum2_default Created                                                                          0.1s
 ✔ Container cs5200_practicum2       Started                                                                          2.7s
PS C:\CS5200_Practicum2> docker compose ps
NAME                IMAGE         COMMAND                  SERVICE   CREATED          STATUS                    PORTS
cs5200_practicum2   postgres:16   "docker-entrypoint.s…"   db        29 seconds ago   Up 26 seconds (healthy)   0.0.0.0:5433->5432/tcp, [::]:5433->5432/tcp
PS C:\CS5200_Practicum2> docker compose exec -T db psql -U cs5200 -d commerce -v ON_ERROR_STOP=1 -f /workspace/sql/01_schema.sql
DROP TABLE
psql:/workspace/sql/01_schema.sql:2: NOTICE:  table "inventory" does not exist, skipping
psql:/workspace/sql/01_schema.sql:2: NOTICE:  table "shipments" does not exist, skipping
psql:/workspace/sql/01_schema.sql:2: NOTICE:  table "payments" does not exist, skipping
psql:/workspace/sql/01_schema.sql:2: NOTICE:  table "order_items" does not exist, skipping
psql:/workspace/sql/01_schema.sql:2: NOTICE:  table "orders" does not exist, skipping
psql:/workspace/sql/01_schema.sql:2: NOTICE:  table "products" does not exist, skipping
psql:/workspace/sql/01_schema.sql:2: NOTICE:  table "categories" does not exist, skipping
psql:/workspace/sql/01_schema.sql:2: NOTICE:  table "warehouses" does not exist, skipping
psql:/workspace/sql/01_schema.sql:2: NOTICE:  table "customers" does not exist, skipping
CREATE TABLE
CREATE TABLE
CREATE TABLE
CREATE TABLE
CREATE TABLE
CREATE TABLE
CREATE TABLE
CREATE TABLE
CREATE TABLE
PS C:\CS5200_Practicum2> docker compose exec -T db psql -U cs5200 -d commerce -v ON_ERROR_STOP=1 -v order_count=10000 -f /workspace/sql/02_seed_data.sql
Loading 10000 orders
SET
SET
INSERT 0 20
INSERT 0 5
INSERT 0 20000
INSERT 0 2000
INSERT 0 10000
INSERT 0 25000
INSERT 0 6000
INSERT 0 4000
INSERT 0 10000
ANALYZE
 table_name  | rows
-------------+-------
 customers   | 20000
 order_items | 25000
 orders      | 10000
 products    |  2000
(4 rows)

PS C:\CS5200_Practicum2> docker compose exec -T db psql -U cs5200 -d commerce -v ON_ERROR_STOP=1 -f /workspace/sql/03_validation.sql
               check_name                | violations
-----------------------------------------+------------
 negative_available_inventory            |          0
 payments_without_paid_or_shipped_orders |          0
 orders_without_items                    |          0
 duplicate_order_line_keys               |          0
(4 rows)

PS C:\CS5200_Practicum2> docker compose exec -T db psql -U cs5200 -d commerce -v ON_ERROR_STOP=1 -f /workspace/queries/team_2.sql
        month        |  revenue
---------------------+-----------
 2024-01-01 00:00:00 | 158731.72
 2024-02-01 00:00:00 | 157087.92
 2024-03-01 00:00:00 | 161258.52
 2024-04-01 00:00:00 | 171674.72
 2024-05-01 00:00:00 | 177917.57
 2024-06-01 00:00:00 | 156850.52
 2024-07-01 00:00:00 | 162929.51
 2024-08-01 00:00:00 | 176004.85
 2024-09-01 00:00:00 | 169508.72
 2024-10-01 00:00:00 | 164853.92
 2024-11-01 00:00:00 | 160095.52
 2024-12-01 00:00:00 | 155440.72
 2025-01-01 00:00:00 | 169782.83
 2025-02-01 00:00:00 | 167847.01
 2025-03-01 00:00:00 | 159749.12
 2025-04-01 00:00:00 | 155814.32
 2025-05-01 00:00:00 | 162673.38
 2025-06-01 00:00:00 | 153872.83
 2025-07-01 00:00:00 | 168701.04
 2025-08-01 00:00:00 | 157729.10
 2025-09-01 00:00:00 | 144733.17
 2025-10-01 00:00:00 | 146484.31
 2025-11-01 00:00:00 | 156879.37
 2025-12-01 00:00:00 | 158739.31
(24 rows)

 category_name | orders_count |  revenue
---------------+--------------+-----------
 Category 5    |         1000 | 387120.00
 Category 1    |          500 | 205400.00
 Category 17   |          500 | 202440.00
 Category 15   |         1000 | 200960.00
 Category 13   |          500 | 199480.00
 Category 9    |          500 | 196520.00
 Category 7    |         1000 | 195040.00
 Category 20   |          500 | 153495.00
 Category 12   |          500 | 149055.00
 Category 8    |          500 | 146835.00
 Category 4    |          500 | 144615.00
 Category 19   |          500 | 101960.00
 Category 10   |         1000 |  98630.00
 Category 3    |          500 |  96040.00
 Category 18   |          500 |  50795.00
 Category 2    |          500 |  49835.00
(16 rows)

 region | active_customers | orders_count
--------+------------------+--------------
 ON     |              971 |          975
 BC     |              966 |          978
 NS     |              966 |          974
 AB     |              965 |          973
 QC     |              965 |          977
(5 rows)

 product_id | product_name | units
------------+--------------+-------
        188 | Product 188  |    60
        708 | Product 708  |    60
        264 | Product 264  |    60
         88 | Product 88   |    60
        204 | Product 204  |    60
       1864 | Product 1864 |    60
       1084 | Product 1084 |    60
       1048 | Product 1048 |    60
       1248 | Product 1248 |    60
       1928 | Product 1928 |    60
       1684 | Product 1684 |    60
       1944 | Product 1944 |    60
       1508 | Product 1508 |    60
       1264 | Product 1264 |    60
        528 | Product 528  |    60
       1748 | Product 1748 |    60
       1428 | Product 1428 |    60
        488 | Product 488  |    60
       1828 | Product 1828 |    60
        384 | Product 384  |    60
       1564 | Product 1564 |    60
        768 | Product 768  |    60
        948 | Product 948  |    60
       1328 | Product 1328 |    60
        664 | Product 664  |    60
        764 | Product 764  |    60
       1608 | Product 1608 |    60
        624 | Product 624  |    60
        444 | Product 444  |    60
       1268 | Product 1268 |    60
       1104 | Product 1104 |    60
        128 | Product 128  |    60
       1848 | Product 1848 |    60
        824 | Product 824  |    60
        344 | Product 344  |    60
        904 | Product 904  |    60
        644 | Product 644  |    60
        524 | Product 524  |    60
       1644 | Product 1644 |    60
       1484 | Product 1484 |    60
       1044 | Product 1044 |    60
       1984 | Product 1984 |    60
       1788 | Product 1788 |    60
       1804 | Product 1804 |    60
       1844 | Product 1844 |    60
       1004 | Product 1004 |    60
         44 | Product 44   |    60
        944 | Product 944  |    60
       1144 | Product 1144 |    60
        448 | Product 448  |    60
(50 rows)
