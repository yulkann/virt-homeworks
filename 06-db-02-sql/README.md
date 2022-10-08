# Домашнее задание к занятию "6.2. SQL"


## Задача 1

Используя docker поднимите инстанс PostgreSQL (версию 12) c 2 volume, 
в который будут складываться данные БД и бэкапы.

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

В БД из задачи 1: 


- предоставьте привилегии на все операции пользователю test-admin-user на таблицы БД test_db
- создайте пользователя test-simple-user  
- предоставьте пользователю test-simple-user права на SELECT/INSERT/UPDATE/DELETE данных таблиц БД test_db



Приведите:
- итоговый список БД после выполнения пунктов выше,

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
            Access method: heap

            test_db=# CREATE TABLE clients (id serial,lastname CHAR(30),country CHAR(30),book int, foreign key (book) references orders (id) );
            CREATE TABLE
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

- список пользователей с правами над таблицами test_db

## Задача 3

Используя SQL синтаксис - наполните таблицы следующими тестовыми данными:

Таблица orders

|Наименование|цена|
|------------|----|
|Шоколад| 10 |
|Принтер| 3000 |
|Книга| 500 |
|Монитор| 7000|
|Гитара| 4000|

Таблица clients

|ФИО|Страна проживания|
|------------|----|
|Иванов Иван Иванович| USA |
|Петров Петр Петрович| Canada |
|Иоганн Себастьян Бах| Japan |
|Ронни Джеймс Дио| Russia|
|Ritchie Blackmore| Russia|

Используя SQL синтаксис:
- вычислите количество записей для каждой таблицы 
- приведите в ответе:
    - запросы 
    - результаты их выполнения.

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
