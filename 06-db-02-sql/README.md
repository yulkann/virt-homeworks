# Домашнее задание к занятию "6.2. SQL"


## Задача 1

Приведите получившуюся команду или docker-compose манифест.

            version: "3.9"

            volumes:
              postgressql_data:
              backup_postgressql_data:

            services:
              postgres:
                container_name: postgres_netology
                image: postgres:12
                environment:
                  POSTGRES_DB: "postgres"
                  POSTGRES_USER: "netologyuser"
                  POSTGRES_PASSWORD: "postgres"
                  PGDATA: "/var/lib/postgresql/data/pgdata"
                volumes:
                  - postgressql_data:/var/lib/postgresql/data
                  - backup_postgressql_data:/backup
                  - ./config:/docker-entrypoint-initdb.d
                ports:
                  - "5432:5432"

            networks:
              postgres:
                driver: bridge

## Задача 2

Приведите:
- итоговый список БД после выполнения пунктов выше,

                test_db=# \l
                                                     List of databases
               Name    |    Owner     | Encoding |  Collate   |   Ctype    |         Access privileges
            -----------+--------------+----------+------------+------------+------------------------------------
             postgres  | netologyuser | UTF8     | en_US.utf8 | en_US.utf8 |
             template0 | netologyuser | UTF8     | en_US.utf8 | en_US.utf8 | =c/netologyuser                   +
                       |              |          |            |            | netologyuser=CTc/netologyuser
             template1 | netologyuser | UTF8     | en_US.utf8 | en_US.utf8 | =c/netologyuser                   +
                       |              |          |            |            | netologyuser=CTc/netologyuser
             test_db   | netologyuser | UTF8     | en_US.utf8 | en_US.utf8 | =Tc/netologyuser                  +
                       |              |          |            |            | netologyuser=CTc/netologyuser     +
                       |              |          |            |            | "test-admin-user"=CTc/netologyuser
            (4 rows)
            
- описание таблиц (describe)

             test_db=# \d+ orders
                                                            Table "public.orders"
             Column |     Type      | Collation | Nullable |              Default               | Storage  | Stats target | Description
            --------+---------------+-----------+----------+------------------------------------+----------+--------------+-------------
             id     | integer       |           | not null | nextval('orders_id_seq'::regclass) | plain    |              |
             name   | character(30) |           |          |                                    | extended |              |
             price  | integer       |           |          |                                    | plain    |              |
            Indexes:
                "orders_id_key" UNIQUE CONSTRAINT, btree (id)
            Referenced by:
                TABLE "clients" CONSTRAINT "clients_book_fkey" FOREIGN KEY (book) REFERENCES orders(id)
            Access method: heap

            test_db=# \d+ clients
                                                                Table "public.clients"
              Column  |     Type      | Collation | Nullable |               Default               | Storage  | Stats target | Description
            ----------+---------------+-----------+----------+-------------------------------------+----------+--------------+-------------
             id       | integer       |           | not null | nextval('clients_id_seq'::regclass) | plain    |              |
             lastname | character(30) |           |          |                                     | extended |              |
             country  | character(30) |           |          |                                     | extended |              |
             book     | integer       |           |          |                                     | plain    |              |
            Foreign-key constraints:
                "clients_book_fkey" FOREIGN KEY (book) REFERENCES orders(id)
            Access method: heap

- SQL-запрос для выдачи списка пользователей с правами над таблицами test_db
            
            test_db=# SELECT * FROM information_schema.table_privileges WHERE table_schema = 'public';
            
- список пользователей с правами над таблицами test_db

               grantor    |     grantee      | table_catalog | table_schema | table_name | privilege_type | is_grantable | with_hierarchy
            --------------+------------------+---------------+--------------+------------+----------------+--------------+----------------
             netologyuser | netologyuser     | test_db       | public       | clients    | INSERT         | YES          | NO
             netologyuser | netologyuser     | test_db       | public       | clients    | SELECT         | YES          | YES
             netologyuser | netologyuser     | test_db       | public       | clients    | UPDATE         | YES          | NO
             netologyuser | netologyuser     | test_db       | public       | clients    | DELETE         | YES          | NO
             netologyuser | netologyuser     | test_db       | public       | clients    | TRUNCATE       | YES          | NO
             netologyuser | netologyuser     | test_db       | public       | clients    | REFERENCES     | YES          | NO
             netologyuser | netologyuser     | test_db       | public       | clients    | TRIGGER        | YES          | NO
             netologyuser | test-simple-user | test_db       | public       | clients    | INSERT         | NO           | NO
             netologyuser | test-simple-user | test_db       | public       | clients    | SELECT         | NO           | YES
             netologyuser | test-simple-user | test_db       | public       | clients    | UPDATE         | NO           | NO
             netologyuser | test-simple-user | test_db       | public       | clients    | DELETE         | NO           | NO
             netologyuser | test-admin-user  | test_db       | public       | clients    | INSERT         | YES          | NO
             netologyuser | test-admin-user  | test_db       | public       | clients    | SELECT         | YES          | YES
             netologyuser | test-admin-user  | test_db       | public       | clients    | UPDATE         | YES          | NO
             netologyuser | test-admin-user  | test_db       | public       | clients    | DELETE         | YES          | NO
             netologyuser | test-admin-user  | test_db       | public       | clients    | TRUNCATE       | YES          | NO
             netologyuser | test-admin-user  | test_db       | public       | clients    | REFERENCES     | YES          | NO
             netologyuser | test-admin-user  | test_db       | public       | clients    | TRIGGER        | YES          | NO
             netologyuser | netologyuser     | test_db       | public       | orders     | INSERT         | YES          | NO
             netologyuser | netologyuser     | test_db       | public       | orders     | SELECT         | YES          | YES
             netologyuser | netologyuser     | test_db       | public       | orders     | UPDATE         | YES          | NO
             netologyuser | netologyuser     | test_db       | public       | orders     | DELETE         | YES          | NO
             netologyuser | netologyuser     | test_db       | public       | orders     | TRUNCATE       | YES          | NO
             netologyuser | netologyuser     | test_db       | public       | orders     | REFERENCES     | YES          | NO
             netologyuser | netologyuser     | test_db       | public       | orders     | TRIGGER        | YES          | NO
             netologyuser | test-simple-user | test_db       | public       | orders     | INSERT         | NO           | NO
             netologyuser | test-simple-user | test_db       | public       | orders     | SELECT         | NO           | YES
             netologyuser | test-simple-user | test_db       | public       | orders     | UPDATE         | NO           | NO
             netologyuser | test-simple-user | test_db       | public       | orders     | DELETE         | NO           | NO
             netologyuser | test-admin-user  | test_db       | public       | orders     | INSERT         | YES          | NO
             netologyuser | test-admin-user  | test_db       | public       | orders     | SELECT         | YES          | YES
             netologyuser | test-admin-user  | test_db       | public       | orders     | UPDATE         | YES          | NO
             netologyuser | test-admin-user  | test_db       | public       | orders     | DELETE         | YES          | NO
             netologyuser | test-admin-user  | test_db       | public       | orders     | TRUNCATE       | YES          | NO
             netologyuser | test-admin-user  | test_db       | public       | orders     | REFERENCES     | YES          | NO
             netologyuser | test-admin-user  | test_db       | public       | orders     | TRIGGER        | YES          | NO
            (36 rows)

## Задача 3


                                    test_db=# SELECT COUNT(*) FROM orders;
                                     count
                                    -------
                                         5
                                    (1 row)

                                    test_db=# SELECT COUNT(*) FROM clients;
                                     count
                                    -------
                                         5
                                    (1 row)

## Задача 4

Приведите SQL-запросы для выполнения данных операций.

                        test_db=# UPDATE clients
                        SET book = (SELECT id FROM orders WHERE name = 'Книга')
                        WHERE lastname = 'Иванов Иван Иванович';
                        UPDATE 1

                        test_db=# UPDATE clients
                        SET book = (SELECT id FROM orders WHERE name = 'Монитор')
                        WHERE lastname = 'Петров Петр Петрович';

                        UPDATE clients
                        SET book = (SELECT id FROM orders WHERE name = 'Гитара')
                        WHERE lastname = 'Иоганн Себастьян Бах';
                        UPDATE 1
                        UPDATE 1


Приведите SQL-запрос для выдачи всех пользователей, которые совершили заказ, а также вывод данного запроса.
 
                         test_db=# SELECT * FROM clients WHERE book IS NOT NULL;
                         id |            lastname            |            country             | book
                        ----+--------------------------------+--------------------------------+------
                          1 | Иванов Иван Иванович           | USA                            |    3
                          2 | Петров Петр Петрович           | Canada                         |    4
                          3 | Иоганн Себастьян Бах           | Japan                          |    5
                        (3 rows)
 


## Задача 5

                                    test_db=# explain SELECT * FROM clients WHERE book IS NOT NULL;
                                                             QUERY PLAN
                                    ------------------------------------------------------------
                                     Seq Scan on clients  (cost=0.00..12.80 rows=279 width=256)
                                       Filter: (book IS NOT NULL)
                                    (2 rows)
                                    
                                    cost - стоимосто операции 
                                    rows - число записей  обработанных 
                                    width -средний размер строк
                                    Filter - фильтр запроса.

## Задача 6

Создайте бэкап БД test_db и поместите его в volume, предназначенный для бэкапов (см. Задачу 1).

            # cd /backup
            # ls
            # pg_dump -U netologyuser test_db > /backup/dump.sql
            # ls
            dump.sql

Остановите контейнер с PostgreSQL (но не удаляйте volumes).

                        root@yulka98356:~/06-db-02-sql/PGSQL2# docker stop a30479e23b1f
                        a30479e23b1f
                        root@yulka98356:~/06-db-02-sql/PGSQL2# docker ps -a
                        CONTAINER ID   IMAGE         COMMAND                  CREATED          STATUS                     PORTS                                                 NAMES
                        a30479e23b1f   postgres:12   "docker-entrypoint.s…"   4 hours ago      Exited (0) 7 seconds ago                                                         postgres_netology



Поднимите новый пустой контейнер с PostgreSQL.

                        root@yulka98356:~/06-db-02-sql/PGSQL2# docker-compose up -d
                        Creating network "pgsql2_default" with the default driver
                        Creating volume "pgsql2_postgressql_data" with default driver
                        Creating volume "pgsql2_backup_postgressql_data" with default driver
                        Creating postgres2 ... done
                        root@yulka98356:~/06-db-02-sql/PGSQL2# docker ps -a
                        CONTAINER ID   IMAGE         COMMAND                  CREATED          STATUS                     PORTS                                                 NAMES
                        5e167d80829f   postgres:12   "docker-entrypoint.s…"   14 seconds ago   Up 13 seconds              5432/tcp, 0.0.0.0:5433->5433/tcp, :::5433->5433/tcp   postgres2
                        a30479e23b1f   postgres:12   "docker-entrypoint.s…"   4 hours ago      Exited (1) 3 minutes ago                                                         postgres_netology

Восстановите БД test_db в новом контейнере.


