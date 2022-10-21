# Домашнее задание к занятию "6.5. Elasticsearch"

## Задача 1

В ответе приведите:
- текст Dockerfile манифеста
          
          ### так как из России не скачивался дистрибутив, предварительно скачала нужный и положила в папку с докерфайлом
          
          
                    root@elastichw:/home/ansible# cat Dockerfile
                    FROM centos:7

                    RUN yum install -y java-11-openjdk wget curl perl-Digest-SHA
                    RUN mkdir /usr/elasticsearch
                    COPY elasticsearch-7.17.3-linux-x86_64.tar.gz /usr/elasticsearch
                    RUN cd /usr/elasticsearch && tar -xzf elasticsearch-7.17.3-linux-x86_64.tar.gz
                    COPY elasticsearch.yml /usr/elasticsearch/elasticsearch-7.17.3/config/elasticsearch.yml
                    RUN groupadd elasticsearch && \
                        useradd elasticsearch -g elasticsearch -p elasticsearch && \
                        cd /opt && \
                        chown -R elasticsearch:elasticsearch /usr/elasticsearch

                    RUN mkdir /var/lib/elasticsearch_data && \
                        chown -R elasticsearch:elasticsearch /var/lib/elasticsearch_data && \
                        mkdir /var/lib/elasticsearch_logs && \
                        chown -R elasticsearch:elasticsearch /var/lib/elasticsearch_logs

                    USER elasticsearch

                    CMD ["/usr/sbin/init"]
                    CMD ["/usr/elasticsearch/elasticsearch-7.17.3/bin/elasticsearch"]

- ссылку на образ в репозитории dockerhub

https://hub.docker.com/u/yulkann/elastic

- ответ `elasticsearch` на запрос пути `/` в json виде


            [elasticsearch@d2c57807d242 /]$ curl -X GET 'http://localhost:9200/'
            {
              "name" : "netology_test",
              "cluster_name" : "netology-devops",
              "cluster_uuid" : "NVsMEOh0RwONE-QOy7-Kvg",
              "version" : {
                "number" : "7.17.3",
                "build_flavor" : "default",
                "build_type" : "tar",
                "build_hash" : "5ad023604c8d7416c9eb6c0eadb62b14e766caff",
                "build_date" : "2022-04-19T08:11:19.070913226Z",
                "build_snapshot" : false,
                "lucene_version" : "8.11.1",
                "minimum_wire_compatibility_version" : "6.8.0",
                "minimum_index_compatibility_version" : "6.0.0-beta1"
              },
              "tagline" : "You Know, for Search"
            }

Подсказки:
- при сетевых проблемах внимательно изучите кластерные и сетевые настройки в elasticsearch.yml
- при некоторых проблемах вам поможет docker директива ulimit
- elasticsearch в логах обычно описывает проблему и пути ее решения
- обратите внимание на настройки безопасности такие как `xpack.security.enabled` 
- если докер образ не запускается и падает с ошибкой 137 в этом случае может помочь настройка `-e ES_HEAP_SIZE`
- при настройке `path` возможно потребуется настройка прав доступа на директорию

Далее мы будем работать с данным экземпляром elasticsearch.

## Задача 2

Ознакомтесь с [документацией](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-create-index.html) 
и добавьте в `elasticsearch` 3 индекса, в соответствии со таблицей:

| Имя | Количество реплик | Количество шард |
|-----|-------------------|-----------------|
| ind-1| 0 | 1 |
| ind-2 | 1 | 2 |
| ind-3 | 2 | 4 |


                              [elasticsearch@d2c57807d242 /]$ curl -X PUT localhost:9200/ind-1 -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 1,  "number_of_replicas": 0 }}'
                              {"acknowledged":true,"shards_acknowledged":true,"index":"ind-1"}
                              [elasticsearch@d2c57807d242 /]$ curl -X PUT localhost:9200/ind-2 -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 2,  "number_of_replicas": 1 }}'
                              {"acknowledged":true,"shards_acknowledged":true,"index":"ind-2"}
                              [elasticsearch@d2c57807d242 /]$ curl -X PUT localhost:9200/ind-3 -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 4,  "number_of_replicas": 2 }}'
                              {"acknowledged":true,"shards_acknowledged":true,"index":"ind-3"}

Получите список индексов и их статусов, используя API и **приведите в ответе** на задание.

                              [elasticsearch@d2c57807d242 /]$ curl -X GET 'http://localhost:9200/_cat/indices?v'

                              health status index            uuid                   pri rep docs.count docs.deleted store.size pri.store.size
                              green  open   .geoip_databases ECGffgY2THqHxSvAk1GbJQ   1   0         41            0       39mb           39mb
                              green  open   ind-1            hpJA921MTIGtZZGLFV9Z2g   1   0          0            0       226b           226b
                              yellow open   ind-3            GBbNhM5MSS6pomcOLgBfbA   4   2          0            0       904b           904b
                              yellow open   ind-2            yBSSLI3BQquQBEzHdmSnwQ   2   1          0            0       452b           452b


Получите состояние кластера `elasticsearch`, используя API.

                              [elasticsearch@d2c57807d242 /]$ curl -X GET 'http://localhost:9200/_cluster/health?pretty'
                              {
                                "cluster_name" : "netology-devops",
                                "status" : "yellow",
                                "timed_out" : false,
                                "number_of_nodes" : 1,
                                "number_of_data_nodes" : 1,
                                "active_primary_shards" : 10,
                                "active_shards" : 10,
                                "relocating_shards" : 0,
                                "initializing_shards" : 0,
                                "unassigned_shards" : 10,
                                "delayed_unassigned_shards" : 0,
                                "number_of_pending_tasks" : 0,
                                "number_of_in_flight_fetch" : 0,
                                "task_max_waiting_in_queue_millis" : 0,
                                "active_shards_percent_as_number" : 50.0
                              }


Как вы думаете, почему часть индексов и кластер находится в состоянии yellow?

                              так как у меня только одна нода и некуда реплицировать

Удалите все индексы.

                              [elasticsearch@d2c57807d242 /]$ curl -X DELETE http://localhost:9200/ind-1
                              {"acknowledged":true}
                              [elasticsearch@d2c57807d242 /]$ curl -X DELETE http://localhost:9200/ind-2
                              {"acknowledged":true}
                              [elasticsearch@d2c57807d242 /]$ curl -X DELETE http://localhost:9200/ind-3
                              {"acknowledged":true}
                              [elasticsearch@d2c57807d242 /]$ curl -XGET 'http://localhost:9200/_cat/indices?v'
                              health status index            uuid                   pri rep docs.count docs.deleted store.size pri.store.size
                              green  open   .geoip_databases ECGffgY2THqHxSvAk1GbJQ   1   0         41            0       39mb           39mb


## Задача 3


Создайте директорию `{путь до корневой директории с elasticsearch в образе}/snapshots`.

Используя API [зарегистрируйте](https://www.elastic.co/guide/en/elasticsearch/reference/current/snapshots-register-repository.html#snapshots-register-repository) 
данную директорию как `snapshot repository` c именем `netology_backup`.

**Приведите в ответе** запрос API и результат вызова API для создания репозитория.

                              [elasticsearch@a407414bbcd3 /]$ curl -XPUT http://localhost:9200/_snapshot/netology_backup?pretty -H 'content-type: application/json' -d'{ "type": "fs", "settings": { "location": "/usr/elasticsearch/elasticsearch-7.17.3/bin/snapshots"}}'
                              {
                                "acknowledged" : true
                              }


Создайте индекс `test` с 0 реплик и 1 шардом и **приведите в ответе** список индексов.

                              [elasticsearch@a407414bbcd3 /]$ curl -XGET 'http://localhost:9200/_cat/indices?v'
                              health status index            uuid                   pri rep docs.count docs.deleted store.size pri.store.size
                              green  open   .geoip_databases sNMlqjvIT_yFxUMOpxK6dA   1   0         41            0       39mb           39mb
                              green  open   test             HKNMjidwTQS5BhxCVql-zQ   1   0          0            0       226b           226b

[Создайте `snapshot`](https://www.elastic.co/guide/en/elasticsearch/reference/current/snapshots-take-snapshot.html) 
состояния кластера `elasticsearch`.

**Приведите в ответе** список файлов в директории со `snapshot`ами.

                              [elasticsearch@a407414bbcd3 /]$ ll /usr/elasticsearch/elasticsearch-7.17.3/bin/snapshots
                              total 48
                              -rw-r--r-- 1 elasticsearch elasticsearch  1426 Oct 21 11:44 index-0
                              -rw-r--r-- 1 elasticsearch elasticsearch     8 Oct 21 11:44 index.latest
                              drwxr-xr-x 6 elasticsearch elasticsearch  4096 Oct 21 11:44 indices
                              -rw-r--r-- 1 elasticsearch elasticsearch 29301 Oct 21 11:44 meta-136yYPnqS0OfwFk86I9oag.dat
                              -rw-r--r-- 1 elasticsearch elasticsearch   713 Oct 21 11:44 snap-136yYPnqS0OfwFk86I9oag.dat

Удалите индекс `test` и создайте индекс `test-2`. **Приведите в ответе** список индексов.

[Восстановите](https://www.elastic.co/guide/en/elasticsearch/reference/current/snapshots-restore-snapshot.html) состояние
кластера `elasticsearch` из `snapshot`, созданного ранее. 

**Приведите в ответе** запрос к API восстановления и итоговый список индексов.

                              [elasticsearch@a407414bbcd3 /]$ curl -XPOST localhost:9200/_snapshot/netology_backup/first_snapshot/_restore?pretty -H 'content-type: application/json' -d'{"indices": "test"}'
                              {
                                "accepted" : true
                              }
                              [elasticsearch@a407414bbcd3 /]$ curl -XGET 'http://localhost:9200/_cat/indices?v'
                              health status index            uuid                   pri rep docs.count docs.deleted store.size pri.store.size
                              green  open   .geoip_databases sNMlqjvIT_yFxUMOpxK6dA   1   0         41            0       39mb           39mb
                              green  open   test-2           x5GGpcKTTGWZ6ZtPyEfbEg   1   0          0            0       226b           226b
                              green  open   test             yF1lINNQSyGxXkIFqmG6Aw   1   0          0            0       226b           226b
