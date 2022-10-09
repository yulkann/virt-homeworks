# Домашнее задание к занятию "7.2. Облачные провайдеры и синтаксис Terraform."

Зачастую разбираться в новых инструментах гораздо интересней понимая то, как они работают изнутри. 
Поэтому в рамках первого *необязательного* задания предлагается завести свою учетную запись в AWS (Amazon Web Services) или Yandex.Cloud.
Идеально будет познакомится с обоими облаками, потому что они отличаются. 


## Задача 1 (Вариант с Yandex.Cloud). Регистрация в ЯО и знакомство с основами (необязательно, но крайне желательно).

 Используйте раздел "Подготовьте облако к работе" для регистрации аккаунта. 
  

           root@yulka98356:/netology-terraform# yc config list
            service-account-key:
              id: (XXXXX)
              service_account_id: (XXXXXX)
              created_at: "2022-10-09T17:44:39.753288673Z"
              key_algorithm: RSA_2048
              public_key: |
                -----BEGIN PUBLIC KEY-----

                -----END PUBLIC KEY-----
              private_key: |
                -----BEGIN PRIVATE KEY-----
            =
                -----END PRIVATE KEY-----
            cloud-id: ($YC_CLOUD_ID)
            folder-id: ($YC_FOLDER_ID)
            
 Далее раздел "Настройте провайдер" для подготовки базового терраформ конфига.
 
            root@yulka98356:/netology-terraform# cat main.tf
            # Provider
            terraform {
              required_providers {
                yandex = {
                  source = "yandex-cloud/yandex"
                }
              }
            }

            provider "yandex" {
              service_account_key_file = "key.json"
              cloud_id  = "${var.yandex_cloud_id}"
              folder_id = "${var.yandex_folder_id}"
              required_version = ">= 0.13"
            }

  Воспользуйтесь инструкцией на сайте терраформа, что бы не указывать авторизационный токен в коде, а терраформ провайдер брал его из переменных окружений.

            export YC_TOKEN=$(yc iam create-token)
            export YC_CLOUD_ID=$(yc config get cloud-id)
            export YC_FOLDER_ID=$(yc config get folder-id)


## Задача 2. Создание aws ec2 или yandex_compute_instance через терраформ. 

1. В каталоге `terraform` вашего основного репозитория, который был создан в начале курсе, создайте файл `main.tf` и `versions.tf`.

            root@yulka98356:/devops-netology/terraform# ll
            total 24
            drwxr-xr-x 2 root root 4096 Oct  9 19:22 ./
            drwxr-xr-x 4 root root 4096 Oct  9 19:19 ../
            -rw-r--r-- 1 root root  861 Oct  9 19:19 .gitignore
            -rw-r--r-- 1 root root  283 Oct  9 19:21 main.tf
            -rw-r--r-- 1 root root  541 Oct  9 19:22 variables.tf
            -rw-r--r-- 1 root root    1 Oct  9 19:22 versions.tf

2. Зарегистрируйте провайдер 

            root@yulka98356:/netology-terraform# cat main.tf
            # Provider
            terraform {
              required_providers {
                yandex = {
                  source = "yandex-cloud/yandex"
                }
              }
            }

            provider "yandex" {
              service_account_key_file = "key.json"
              cloud_id  = "${var.yandex_cloud_id}"
              folder_id = "${var.yandex_folder_id}"
              required_version = ">= 0.13"
            }

3. Внимание! В гит репозиторий нельзя пушить ваши личные ключи доступа к аккаунту. Поэтому в предыдущем задании мы указывали
их в виде переменных окружения. 

             root@yulka98356:/devops-netology/terraform# cat variables.tf
             # Заменить на ID своего облака
             # https://console.cloud.yandex.ru/cloud?section=overview
             variable "yandex_cloud_id" {
               default = $YC_CLOUD_ID
             }

             # Заменить на Folder своего облака
             # https://console.cloud.yandex.ru/cloud?section=overview
             variable "yandex_folder_id" {
               default = $YC_FOLDER_ID
             }

             # Заменить на ID своего образа
             # ID можно узнать с помощью команды yc compute image list
             variable "centos-7-base" {
               default = "fd87e6142mvinu7q1kul"

4. В файле `main.tf` воспользуйтесь блоком `data "aws_ami` для поиска ami образа последнего Ubuntu.  

               image_id fd8kdq6d0p8sij7h5qe3

5. В файле `main.tf` создайте рессурс 
   1. либо [yandex_compute_image](https://registry.terraform.io/providers/yandex-cloud/yandex/latest/docs/resources/compute_image).

7. Если вы выполнили первый пункт, то добейтесь того, что бы команда `terraform plan` выполнялась без ошибок. 

                Plan: 3 to add, 0 to change, 0 to destroy.

                Changes to Outputs:
                  + external_ip_address_vm_1 = (known after apply)
                  + internal_ip_address_vm_1 = (known after apply)

                ────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

                Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly these actions if you run "terraform apply" now.
                root@yulka98356:/devops-netology/terraform#

В качестве результата задания предоставьте:
1. Ответ на вопрос: при помощи какого инструмента (из разобранных на прошлом занятии) можно создать свой образ ami?

          Packet

1. Ссылку на репозиторий с исходной конфигурацией терраформа.  

          https://github.com/yulkann/devops-netology
 
---

### Как cдавать задание

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---
