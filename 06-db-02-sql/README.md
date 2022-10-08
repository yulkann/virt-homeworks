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

Часть пользователей из таблицы clients решили оформить заказы из таблицы orders.

Используя foreign keys свяжите записи из таблиц, согласно таблице:

|ФИО|Заказ|
|------------|----|
|Иванов Иван Иванович| Книга |
|Петров Петр Петрович| Монитор |
|Иоганн Себастьян Бах| Гитара |

Приведите SQL-запросы для выполнения данных операций.

Приведите SQL-запрос для выдачи всех пользователей, которые совершили заказ, а также вывод данного запроса.
 
Подсказк - используйте директиву `UPDATE`.

## Задача 5

Получите полную информацию по выполнению запроса выдачи всех пользователей из задачи 4 
(используя директиву EXPLAIN).

Приведите получившийся результат и объясните что значат полученные значения.

## Задача 6

Создайте бэкап БД test_db и поместите его в volume, предназначенный для бэкапов (см. Задачу 1).

Остановите контейнер с PostgreSQL (но не удаляйте volumes).

Поднимите новый пустой контейнер с PostgreSQL.

Восстановите БД test_db в новом контейнере.

Приведите список операций, который вы применяли для бэкапа данных и восстановления. 

---

### Как cдавать задание

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---
