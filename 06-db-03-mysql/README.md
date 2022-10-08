# Домашнее задание к занятию "6.3. MySQL"

## Задача 1

Используя docker поднимите инстанс MySQL (версию 8). Данные БД сохраните в volume.

Изучите [бэкап БД](https://github.com/netology-code/virt-homeworks/tree/master/06-db-03-mysql/test_data) и 
восстановитесь из него.

Перейдите в управляющую консоль `mysql` внутри контейнера.

Используя команду `\h` получите список управляющих команд.

Найдите команду для выдачи статуса БД и **приведите в ответе** из ее вывода версию сервера БД.

        mysql> \s
        --------------
        mysql  Ver 8.0.30 for Linux on x86_64 (MySQL Community Server - GPL)

        Connection id:          17
        Current database:       mysql
        Current user:           root@localhost
        SSL:                    Not in use
        Current pager:          stdout
        Using outfile:          ''
        Using delimiter:        ;
        Server version:         8.0.30 MySQL Community Server - GPL
        Protocol version:       10
        Connection:             Localhost via UNIX socket
        Server characterset:    utf8mb4
        Db     characterset:    utf8mb4
        Client characterset:    latin1
        Conn.  characterset:    latin1
        UNIX socket:            /var/run/mysqld/mysqld.sock
        Binary data as:         Hexadecimal
        Uptime:                 23 min 49 sec

        Threads: 2  Questions: 209  Slow queries: 0  Opens: 313  Flush tables: 3  Open tables: 231  Queries per second avg: 0.146
        --------------

Подключитесь к восстановленной БД и получите список таблиц из этой БД.

        mysql> use test_db
        Reading table information for completion of table and column names
        You can turn off this feature to get a quicker startup with -A

        Database changed
        mysql> show tables
            -> ^C
        mysql> show tables ;
        +-------------------+
        | Tables_in_test_db |
        +-------------------+
        | orders            |
        +-------------------+
        1 row in set (0.00 sec)

**Приведите в ответе** количество записей с `price` > 300.

mysql> select count(*) from orders where price >300;
+----------+
| count(*) |
+----------+
|        1 |
+----------+
1 row in set (0.00 sec)


## Задача 2

Создайте пользователя test в БД c паролем test-pass, используя:
- плагин авторизации mysql_native_password
- срок истечения пароля - 180 дней 
- количество попыток авторизации - 3 
- максимальное количество запросов в час - 100
- аттрибуты пользователя:
    - Фамилия "Pretty"
    - Имя "James"

Предоставьте привелегии пользователю `test` на операции SELECT базы `test_db`.
    
Используя таблицу INFORMATION_SCHEMA.USER_ATTRIBUTES получите данные по пользователю `test` и 
**приведите в ответе к задаче**.

## Задача 3

Установите профилирование `SET profiling = 1`.
Изучите вывод профилирования команд `SHOW PROFILES;`.

Исследуйте, какой `engine` используется в таблице БД `test_db` и **приведите в ответе**.

Измените `engine` и **приведите время выполнения и запрос на изменения из профайлера в ответе**:
- на `MyISAM`
- на `InnoDB`

## Задача 4 

Изучите файл `my.cnf` в директории /etc/mysql.

Измените его согласно ТЗ (движок InnoDB):
- Скорость IO важнее сохранности данных
- Нужна компрессия таблиц для экономии места на диске
- Размер буффера с незакомиченными транзакциями 1 Мб
- Буффер кеширования 30% от ОЗУ
- Размер файла логов операций 100 Мб

Приведите в ответе измененный файл `my.cnf`.

---

### Как оформить ДЗ?

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---
