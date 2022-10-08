# Домашнее задание к занятию "6.4. PostgreSQL"

## Задача 1

    \l - список БД
    \c[onnect] {[DBNAME|- USER|- HOST|- PORT|-] | conninfo} connect to new database (currently "postgres") - подключение к БД
    \dt[S+] [PATTERN] - список таблиц
    \d[S+]  NAME - описание содержимого таблиц
    \q - выход из psql

## Задача 2

Используя `psql` создайте БД `test_database`.

    root@yulka98356:/# psql -U postgres
    psql (12.12 (Debian 12.12-1.pgdg110+1))
    Type "help" for help.

    postgres=# CREATE DATABASE test_database ;
    CREATE DATABASE

Изучите [бэкап БД](https://github.com/netology-code/virt-homeworks/tree/master/06-db-04-postgresql/test_data).

Восстановите бэкап БД в `test_database`.

      root@yulka98356:/# psql -U postgres -f /backup/test_dump.sql test_database

Перейдите в управляющую консоль `psql` внутри контейнера.

      root@yulka98356:/# psql -U postgres

Подключитесь к восстановленной БД и проведите операцию ANALYZE для сбора статистики по таблице.

    postgres=# \c test_database
    You are now connected to database "test_database" as user "postgres".


Используя таблицу [pg_stats](https://postgrespro.ru/docs/postgresql/12/view-pg-stats), найдите столбец таблицы `orders` 
с наибольшим средним значением размера элементов в байтах.

**Приведите в ответе** команду, которую вы использовали для вычисления и полученный результат.

    postgres=# \c test_database
    You are now connected to database "test_database" as user "postgres".
    test_database=# \dt
             List of relations
     Schema |  Name  | Type  |  Owner
    --------+--------+-------+----------
     public | orders | table | postgres
    (1 row)

    test_database=# ANALYZE VERBOSE public.orders;
    INFO:  analyzing "public.orders"
    INFO:  "orders": scanned 1 of 1 pages, containing 8 live rows and 0 dead rows; 8 rows in sample, 8 estimated total rows
    ANALYZE
    
     test_database=# SELECT attname, avg_width FROM pg_stats WHERE tablename = 'orders';
     attname | avg_width
    ---------+-----------
     id      |         4
     title   |        16
     price   |         4
    (3 rows)

  **title**

## Задача 3

Архитектор и администратор БД выяснили, что ваша таблица orders разрослась до невиданных размеров и
поиск по ней занимает долгое время. Вам, как успешному выпускнику курсов DevOps в нетологии предложили
провести разбиение таблицы на 2 (шардировать на orders_1 - price>499 и orders_2 - price<=499).

Предложите SQL-транзакцию для проведения данной операции.

Можно ли было изначально исключить "ручное" разбиение при проектировании таблицы orders?

## Задача 4

Используя утилиту `pg_dump` создайте бекап БД `test_database`.

Как бы вы доработали бэкап-файл, чтобы добавить уникальность значения столбца `title` для таблиц `test_database`?

---

### Как cдавать задание

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---
