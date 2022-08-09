
# Домашнее задание к занятию "5.2. Применение принципов IaaC в работе с виртуальными машинами"


## Задача 1

- Опишите своими словами основные преимущества применения на практике IaaC паттернов.

      скорость, масштабируемость, безопасность, восстановление при авариях, минимизация ошибок при развётрывании

- Какой из принципов IaaC является основополагающим?
      
      Идемпотентность

## Задача 2

- Чем Ansible выгодно отличается от других систем управление конфигурациями?

      Ansible фокусируется на оптимизации и скорости, и не требует установки агентов на управляемые узлы — все функции производятся по SSH. Ansible написан на python.

- Какой, на ваш взгляд, метод работы систем конфигурации более надёжный push или pull?

      Я считаю что Push более надежен, так как при развёртывании серверов очень просто автоматизируется и вписывается в философию IaaC, так же простая архитектура.

## Задача 3

Установить на личный компьютер:

- VirtualBox
-
           root@yulka92842:~# vboxmanage --version
           6.1.34_Ubuntur150636

- Vagrant

           root@yulka92842:~# vagrant -v
           Vagrant 2.2.6

- Ansible

            root@yulka92842:~# ansible --version
            ansible 2.9.6
              config file = /etc/ansible/ansible.cfg
              configured module search path = ['/root/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
              ansible python module location = /usr/lib/python3/dist-packages/ansible
              executable location = /usr/bin/ansible
              python version = 3.8.10 (default, Jun 22 2022, 20:18:18) [GCC 9.4.0]


*Приложить вывод команд установленных версий каждой из программ, оформленный в markdown.*

