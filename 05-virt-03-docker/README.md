
# Домашнее задание к занятию "5.3. Введение. Экосистема. Архитектура. Жизненный цикл Docker контейнера"

## Задача 1

Сценарий выполения задачи:

- создайте свой репозиторий на https://hub.docker.com;
- выберете любой образ, который содержит веб-сервер Nginx;
- создайте свой fork образа;
- реализуйте функциональность:
запуск веб-сервера в фоне с индекс-страницей, содержащей HTML-код ниже:
```
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I’m DevOps Engineer!</h1>
</body>
</html>
```
Опубликуйте созданный форк в своем репозитории и предоставьте ответ в виде ссылки на https://hub.docker.com/username_repo.

![изображение](https://user-images.githubusercontent.com/91043924/194444015-ecc9753e-78c7-45a5-bad9-5ee2187fbd45.png)

https://hub.docker.com/r/yulkann/nginx_netology/


## Задача 2

Посмотрите на сценарий ниже и ответьте на вопрос:
"Подходит ли в этом сценарии использование Docker контейнеров или лучше подойдет виртуальная машина, физическая машина? Может быть возможны разные варианты?"

Детально опишите и обоснуйте свой выбор.

--

Сценарий:

- Высоконагруженное монолитное java веб-приложение - больше подходит отдельная виртуальная машина или отдельный физический сервер, так как приложеие высоонагруженное. Контейнеры не подходят, так как предполагаетс что 1 контейнер- 1 сервис
- Nodejs веб-приложение - подходит
- Мобильное приложение c версиями для Android и iOS - виртуалка
- Шина данных на базе Apache Kafka - подходит
- Elasticsearch кластер для реализации логирования продуктивного веб-приложения - три ноды elasticsearch, два logstash и две ноды kibana - подходит
- Мониторинг-стек на базе Prometheus и Grafana-подходит если несколько контейнеров
- MongoDB, как основное хранилище данных для java-приложения - виртуалка
- Gitlab сервер для реализации CI/CD процессов и приватный (закрытый) Docker Registry- физический сервер или виртуалка

## Задача 3

      root@yulka98356:~/05-virt-03-docker# docker run -d -v /data:/data centos:centos7.9.2009 sleep infinity
      b19750d5288c7479caf5f198fe9c7ccf84a6b4d100e3652933bd602c7f888351
      root@yulka98356:~/05-virt-03-docker# docker ps
      CONTAINER ID   IMAGE                   COMMAND            CREATED          STATUS         PORTS     NAMES
      b19750d5288c   centos:centos7.9.2009   "sleep infinity"   10 seconds ago   Up 9 seconds             quizzical_maxwell
      root@yulka98356:~/05-virt-03-docker# docker exec -it b19750d5288c ls -lah
      total 68K
      drwxr-xr-x   1 root root 4.0K Oct  7 02:08 .
      drwxr-xr-x   1 root root 4.0K Oct  7 02:08 ..
      -rwxr-xr-x   1 root root    0 Oct  7 02:08 .dockerenv
      -rw-r--r--   1 root root  12K Nov 13  2020 anaconda-post.log
      lrwxrwxrwx   1 root root    7 Nov 13  2020 bin -> usr/bin
      drwxr-xr-x   2 root root 4.0K Oct  7 01:34 data
      drwxr-xr-x   5 root root  340 Oct  7 02:08 dev
      drwxr-xr-x   1 root root 4.0K Oct  7 02:08 etc
      drwxr-xr-x   2 root root 4.0K Apr 11  2018 home
      lrwxrwxrwx   1 root root    7 Nov 13  2020 lib -> usr/lib
      lrwxrwxrwx   1 root root    9 Nov 13  2020 lib64 -> usr/lib64
      drwxr-xr-x   2 root root 4.0K Apr 11  2018 media
      drwxr-xr-x   2 root root 4.0K Apr 11  2018 mnt
      drwxr-xr-x   2 root root 4.0K Apr 11  2018 opt
      dr-xr-xr-x 252 root root    0 Oct  7 02:08 proc
      dr-xr-x---   2 root root 4.0K Nov 13  2020 root
      drwxr-xr-x  11 root root 4.0K Nov 13  2020 run
      lrwxrwxrwx   1 root root    8 Nov 13  2020 sbin -> usr/sbin
      drwxr-xr-x   2 root root 4.0K Apr 11  2018 srv
      dr-xr-xr-x  13 root root    0 Oct  7 02:08 sys
      drwxrwxrwt   7 root root 4.0K Nov 13  2020 tmp
      drwxr-xr-x  13 root root 4.0K Nov 13  2020 usr
      drwxr-xr-x  18 root root 4.0K Nov 13  2020 var
      root@yulka98356:~/05-virt-03-docker# docker run -v /data:/data -d debian sleep infinity
      Unable to find image 'debian:latest' locally
      latest: Pulling from library/debian
      f606d8928ed3: Pull complete
      Digest: sha256:e538a2f0566efc44db21503277c7312a142f4d0dedc5d2886932b92626104bff
      Status: Downloaded newer image for debian:latest
      835da72454bc91082aacfb61116178ac1ed1b4881f9da34989ac605954ad65be
      root@yulka98356:~/05-virt-03-docker# docker ps
      CONTAINER ID   IMAGE                   COMMAND            CREATED          STATUS          PORTS     NAMES
      835da72454bc   debian                  "sleep infinity"   15 seconds ago   Up 13 seconds             jovial_easley
      b19750d5288c   centos:centos7.9.2009   "sleep infinity"   2 minutes ago    Up 2 minutes              quizzical_maxwell
      root@yulka98356:~/05-virt-03-docker# docker exec -it 835da72454bc ls -lah
      total 76K
      drwxr-xr-x   1 root root 4.0K Oct  7 02:11 .
      drwxr-xr-x   1 root root 4.0K Oct  7 02:11 ..
      -rwxr-xr-x   1 root root    0 Oct  7 02:11 .dockerenv
      drwxr-xr-x   2 root root 4.0K Oct  4 00:00 bin
      drwxr-xr-x   2 root root 4.0K Sep  3 12:10 boot
      drwxr-xr-x   2 root root 4.0K Oct  7 01:34 data
      drwxr-xr-x   5 root root  340 Oct  7 02:11 dev
      drwxr-xr-x   1 root root 4.0K Oct  7 02:11 etc
      drwxr-xr-x   2 root root 4.0K Sep  3 12:10 home
      drwxr-xr-x   8 root root 4.0K Oct  4 00:00 lib
      drwxr-xr-x   2 root root 4.0K Oct  4 00:00 lib64
      drwxr-xr-x   2 root root 4.0K Oct  4 00:00 media
      drwxr-xr-x   2 root root 4.0K Oct  4 00:00 mnt
      drwxr-xr-x   2 root root 4.0K Oct  4 00:00 opt
      dr-xr-xr-x 254 root root    0 Oct  7 02:11 proc
      drwx------   2 root root 4.0K Oct  4 00:00 root
      drwxr-xr-x   3 root root 4.0K Oct  4 00:00 run
      drwxr-xr-x   2 root root 4.0K Oct  4 00:00 sbin
      drwxr-xr-x   2 root root 4.0K Oct  4 00:00 srv
      dr-xr-xr-x  13 root root    0 Oct  7 02:11 sys
      drwxrwxrwt   2 root root 4.0K Oct  4 00:00 tmp
      drwxr-xr-x  11 root root 4.0K Oct  4 00:00 usr
      drwxr-xr-x  11 root root 4.0K Oct  4 00:00 var
      root@yulka98356:~/05-virt-03-docker# docker exec -it b19750d5288c echo '' > /data/FILE1
      root@yulka98356:~/05-virt-03-docker# docker exec -it 835da72454bc echo '' > /data/FILE2
      root@yulka98356:~/05-virt-03-docker# echo '' > /data/FILE3
      root@yulka98356:~/05-virt-03-docker# docker exec -it 835da72454bc ls -lah /data
      total 20K
      drwxr-xr-x 2 root root 4.0K Oct  7 02:13 .
      drwxr-xr-x 1 root root 4.0K Oct  7 02:11 ..
      -rw-r--r-- 1 root root    2 Oct  7 02:13 FILE1
      -rw-r--r-- 1 root root    2 Oct  7 02:13 FILE2
      -rw-r--r-- 1 root root    1 Oct  7 02:13 FILE3

## Задача 4 (*)

Воспроизвести практическую часть лекции самостоятельно.

Соберите Docker образ с Ansible, загрузите на Docker Hub и пришлите ссылку вместе с остальными ответами к задачам.



