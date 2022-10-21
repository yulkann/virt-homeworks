# Домашнее задание к занятию "6.5. Elasticsearch"

## Задача 1

В этом задании вы потренируетесь в:
- установке elasticsearch
- первоначальном конфигурировании elastcisearch
- запуске elasticsearch в docker

Используя докер образ [elasticsearch:7](https://hub.docker.com/_/elasticsearch) как базовый:

- составьте Dockerfile-манифест для elasticsearch
- соберите docker-образ и сделайте `push` в ваш docker.io репозиторий
- запустите контейнер из получившегося образа и выполните запрос пути `/` c хост-машины

Требования к `elasticsearch.yml`:
- данные `path` должны сохраняться в `/var/lib` 
- имя ноды должно быть `netology_test`

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

В этом задании вы научитесь:
- создавать и удалять индексы
- изучать состояние кластера
- обосновывать причину деградации доступности данных

Ознакомтесь с [документацией](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-create-index.html) 
и добавьте в `elasticsearch` 3 индекса, в соответствии со таблицей:

| Имя | Количество реплик | Количество шард |
|-----|-------------------|-----------------|
| ind-1| 0 | 1 |
| ind-2 | 1 | 2 |
| ind-3 | 2 | 4 |

Получите список индексов и их статусов, используя API и **приведите в ответе** на задание.

Получите состояние кластера `elasticsearch`, используя API.

Как вы думаете, почему часть индексов и кластер находится в состоянии yellow?

Удалите все индексы.

**Важно**

При проектировании кластера elasticsearch нужно корректно рассчитывать количество реплик и шард,
иначе возможна потеря данных индексов, вплоть до полной, при деградации системы.

## Задача 3

В данном задании вы научитесь:
- создавать бэкапы данных
- восстанавливать индексы из бэкапов

Создайте директорию `{путь до корневой директории с elasticsearch в образе}/snapshots`.

Используя API [зарегистрируйте](https://www.elastic.co/guide/en/elasticsearch/reference/current/snapshots-register-repository.html#snapshots-register-repository) 
данную директорию как `snapshot repository` c именем `netology_backup`.

**Приведите в ответе** запрос API и результат вызова API для создания репозитория.

Создайте индекс `test` с 0 реплик и 1 шардом и **приведите в ответе** список индексов.

[Создайте `snapshot`](https://www.elastic.co/guide/en/elasticsearch/reference/current/snapshots-take-snapshot.html) 
состояния кластера `elasticsearch`.

**Приведите в ответе** список файлов в директории со `snapshot`ами.

Удалите индекс `test` и создайте индекс `test-2`. **Приведите в ответе** список индексов.

[Восстановите](https://www.elastic.co/guide/en/elasticsearch/reference/current/snapshots-restore-snapshot.html) состояние
кластера `elasticsearch` из `snapshot`, созданного ранее. 

**Приведите в ответе** запрос к API восстановления и итоговый список индексов.

Подсказки:
- возможно вам понадобится доработать `elasticsearch.yml` в части директивы `path.repo` и перезапустить `elasticsearch`

---

### Как cдавать задание

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---
