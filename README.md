# Домашнее задание к занятию "7.1. Инфраструктура как код"

## Задача 1. Выбор инструментов. 
 
### Легенда
 
Через час совещание на котором менеджер расскажет о новом проекте. Начать работу над которым надо 
будет уже сегодня. 
На данный момент известно, что это будет сервис, который ваша компания будет предоставлять внешним заказчикам.
Первое время, скорее всего, будет один внешний клиент, со временем внешних клиентов станет больше.

Так же по разговорам в компании есть вероятность, что техническое задание еще не четкое, что приведет к большому
количеству небольших релизов, тестирований интеграций, откатов, доработок, то есть скучно не будет.  
   
Вам, как девопс инженеру, будет необходимо принять решение об инструментах для организации инфраструктуры.
На данный момент в вашей компании уже используются следующие инструменты: 
- остатки Сloud Formation, 
- некоторые образы сделаны при помощи Packer,
- год назад начали активно использовать Terraform, 
- разработчики привыкли использовать Docker, 
- уже есть большая база Kubernetes конфигураций, 
- для автоматизации процессов используется Teamcity, 
- также есть совсем немного Ansible скриптов, 
- и ряд bash скриптов для упрощения рутинных задач.  

Для этого в рамках совещания надо будет выяснить подробности о проекте, что бы в итоге определиться с инструментами:

1. Какой тип инфраструктуры будем использовать для этого проекта: изменяемый или не изменяемый?
1. Будет ли центральный сервер для управления инфраструктурой?
1. Будут ли агенты на серверах?
1. Будут ли использованы средства для управления конфигурацией или инициализации ресурсов? 
 
В связи с тем, что проект стартует уже сегодня, в рамках совещания надо будет определиться со всеми этими вопросами.

### В результате задачи необходимо

1. Ответить на четыре вопроса представленных в разделе "Легенда". 
1. Какие инструменты из уже используемых вы хотели бы использовать для нового проекта? 
1. Хотите ли рассмотреть возможность внедрения новых инструментов для этого проекта? 

Если для ответа на эти вопросы недостаточно информации, то напишите какие моменты уточните на совещании.

Решение:

Резюмируем:
- много изменений, большое кол-во релизов.
- один клиент, потом еще,точно понадобится масштабирование понадобится
- Есть опыт в Cloud Formation Terraform Ansible
- Много испольуют Docker и Kuber
- Есть bash скрипты 
- Архитектура будущего приложения скорее всего будет микросервисной, т.к. привыкли к Docker.

Будем использовать:
- Teamcity как CI\CD 
- Dockers и Kuber, так как есть экспертиза его использования.
- Packer  в случае неизменяемой инфраструктуры.
- Ansible как универсальное средство управления конфигурациями для любого типа инфраструктуры. 
- Terraform инициализации хостов для облачной инфраструктуры или для инициализации виртуальных хостов при on-prem инфраструктуре.
- 

Какие еще вопросы надо задать:

1. Будет использоваться облачная инфраструктура или on-prem
2. Есть ли архитектурная схема приложения? Какие компонеты в данной архитектуре? Предполагается ли использовать базы данных,брокеры сообщений, кеширование, медиасервера, ElasticSearch
 и т.д.
3. Какие требования к нагрузке? И в целом к приложению особенно в части теоремы CAP, PACELC (консистентность, доступность, время отклика, устойчивость к разделению)?
4. Требования к мониторингу работы инфраструктуры нового приложения и собственно работы самого приложения.
5. Чтобы принять решение об изменяемой или неизменяемой инфраструктуре, нужны ответы на следующие вопросы:
- допускается ли более длительный downtime в случае неизменяемой инфраструктуры,
- сервисы приложений будут stateless или предполагается некое хранение ими каких-то данных
- готов ли бюджет проекта нести дополнительные расходы на автоматизацию СI/CD в случае неизменяемой инфраструктуры

Добавил бы использование ELK - elasticsearch, logstash, kibana  для мониторинга. 

Центральный сервер использовать бы стал, если кол-во хостов, которые необходимо обновлять бы более двух.
Т.к. мы используем terraform и ansible, то установка агентов на хосты не нужна.


## Задача 2. Установка терраформ. 

Официальный сайт: https://www.terraform.io/

Установите терраформ при помощи менеджера пакетов используемого в вашей операционной системе.
В виде результата этой задачи приложите вывод команды `terraform --version`.
```
vagrant@vagrant:~$ sudo apt update && sudo apt install terraform
Hit:7 https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 Release
Hit:9 https://download.docker.com/linux/ubuntu focal InRelease
Get:10 http://us.archive.ubuntu.com/ubuntu focal-updates InRelease [114 kB]
Get:11 http://us.archive.ubuntu.com/ubuntu focal-backports InRelease [108 kB]
Get:12 http://us.archive.ubuntu.com/ubuntu focal-security InRelease [114 kB]
Get:13 http://us.archive.ubuntu.com/ubuntu focal-updates/main amd64 Packages [2,113 kB]
Get:13 http://us.archive.ubuntu.com/ubuntu focal-updates/main amd64 Packages [2,113 kB]
Get:14 http://us.archive.ubuntu.com/ubuntu focal-updates/main Translation-en [375 kB]
Get:15 http://us.archive.ubuntu.com/ubuntu focal-updates/universe amd64 Packages [955 kB]
Get:16 http://us.archive.ubuntu.com/ubuntu focal-updates/universe Translation-en [219 kB]
Get:17 http://us.archive.ubuntu.com/ubuntu focal-backports/universe amd64 Packages [23.9 kB]
Get:18 http://us.archive.ubuntu.com/ubuntu focal-security/main amd64 Packages [1,740 kB]
Get:18 http://us.archive.ubuntu.com/ubuntu focal-security/main amd64 Packages [1,740 kB]
Get:19 http://us.archive.ubuntu.com/ubuntu focal-security/main Translation-en [292 kB]
Get:20 http://us.archive.ubuntu.com/ubuntu focal-security/universe amd64 Packages [728 kB]
Get:21 http://us.archive.ubuntu.com/ubuntu focal-security/universe Translation-en [134 kB]
Fetched 5,076 kB in 38s (133 kB/s)
Reading package lists... Done
Building dependency tree
Reading state information... Done
105 packages can be upgraded. Run 'apt list --upgradable' to see them.
W: Target Packages (main/binary-amd64/Packages) is configured multiple times in /etc/apt/sources.list:50 and /etc/apt/sources.list.d/hashicorp.list:1
W: Target Packages (main/binary-all/Packages) is configured multiple times in /etc/apt/sources.list:50 and /etc/apt/sources.list.d/hashicorp.list:1
W: Target Translations (main/i18n/Translation-en_US) is configured multiple times in /etc/apt/sources.list:50 and /etc/apt/sources.list.d/hashicorp.list:1
W: Target Translations (main/i18n/Translation-en) is configured multiple times in /etc/apt/sources.list:50 and /etc/apt/sources.list.d/hashicorp.list:1
W: Target Packages (main/binary-amd64/Packages) is configured multiple times in /etc/apt/sources.list:50 and /etc/apt/sources.list.d/hashicorp.list:1
W: Target Packages (main/binary-all/Packages) is configured multiple times in /etc/apt/sources.list:50 and /etc/apt/sources.list.d/hashicorp.list:1
W: Target Translations (main/i18n/Translation-en_US) is configured multiple times in /etc/apt/sources.list:50 and /etc/apt/sources.list.d/hashicorp.list:1
W: Target Translations (main/i18n/Translation-en) is configured multiple times in /etc/apt/sources.list:50 and /etc/apt/sources.list.d/hashicorp.list:1
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following packages were automatically installed and are no longer required:
  libfile-basedir-perl libfile-desktopentry-perl libfile-mimeinfo-perl libfontenc1 libfwupdplugin1 libio-stringy-perl libipc-system-simple-perl libnet-dbus-perl libtie-ixhash-perl libu2f-udev
  libx11-protocol-perl libxft2 libxkbfile1 libxml-parser-perl libxml-twig-perl libxml-xpathengine-perl libxmuu1 libxv1 libxxf86dga1 linux-image-5.4.0-91-generic linux-modules-5.4.0-91-generic
  linux-modules-extra-5.4.0-91-generic x11-utils x11-xserver-utils xdg-utils
Use 'sudo apt autoremove' to remove them.
The following NEW packages will be installed:
  terraform
0 upgraded, 1 newly installed, 0 to remove and 105 not upgraded.
Need to get 19.5 MB of archives.
After this operation, 61.2 MB of additional disk space will be used.
Get:1 https://apt.releases.hashicorp.com focal/main amd64 terraform amd64 1.3.0 [19.5 MB]
Fetched 19.5 MB in 2min 9s (151 kB/s)
Selecting previously unselected package terraform.
(Reading database ... 201320 files and directories currently installed.)
Preparing to unpack .../terraform_1.3.0_amd64.deb ...
Unpacking terraform (1.3.0) ...
Setting up terraform (1.3.0) ...

vagrant@vagrant:~$ terraform --version
Terraform v1.2.6
on linux_amd64

Your version of Terraform is out of date! The latest version
is 1.3.0. You can update by downloading from https://www.terraform.io/downloads.html
```
## Задача 3. Поддержка легаси кода. 

В какой-то момент вы обновили терраформ до новой версии, например с 0.12 до 0.13. 
А код одного из проектов настолько устарел, что не может работать с версией 0.13. 
В связи с этим необходимо сделать так, чтобы вы могли одновременно использовать последнюю версию терраформа установленную при помощи
штатного менеджера пакетов и устаревшую версию 0.12. 

В виде результата этой задачи приложите вывод `--version` двух версий терраформа доступных на вашем компьютере 
или виртуальной машине.

Решение:
- посмотрим какие версии доступны:
```
vagrant@vagrant:/usr/share/doc/apt/examples$ sudo apt policy terraform
terraform:
  Installed: (none)
  Candidate: 1.3.0
  Version table:
     1.3.0 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     1.2.9 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     1.2.8 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     1.2.7 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     1.2.6 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     1.2.5 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     1.2.4 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     1.2.3 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     1.2.2 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     1.2.1 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     1.2.0 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     1.1.9 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     1.1.8 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     1.1.7 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     1.1.6 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     1.1.5 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     1.1.4 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     1.1.3 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     1.1.2 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     1.1.1 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     1.1.0 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     1.0.11 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     1.0.10 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     1.0.9 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     1.0.8 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     1.0.7 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     1.0.6 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     1.0.5 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     1.0.4 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     1.0.3 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     1.0.2 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     1.0.1 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     1.0.0 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.15.5 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.15.4 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.15.3 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.15.2 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.15.1 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.15.0 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.14.11 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.14.10 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.14.9 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.14.8 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.14.7 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.14.6 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.14.5 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.14.4 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.14.3 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.14.2 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.14.1 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.14.0 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.13.7 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.13.6 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.13.5 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.13.4-2 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.13.4 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.13.3-2 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.13.3 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.13.2 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.13.1 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.13.0 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.12.31 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.12.30 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.12.29 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.12.28 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.12.27 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.12.26 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.11.15 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
     0.11.14 500
        500 https://apt.releases.hashicorp.com focal/main amd64 Packages
```
установим нужные версии, сначала через пакетный менеджер:

```
vagrant@vagrant:~$ sudo apt install terraform=0.13.7
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following packages were automatically installed and are no longer required:
  libfile-basedir-perl libfile-desktopentry-perl libfile-mimeinfo-perl libfontenc1 libfwupdplugin1 libio-stringy-perl libipc-system-simple-perl libnet-dbus-perl libtie-ixhash-perl libu2f-udev
  libx11-protocol-perl libxft2 libxkbfile1 libxml-parser-perl libxml-twig-perl libxml-xpathengine-perl libxmuu1 libxv1 libxxf86dga1 linux-image-5.4.0-91-generic linux-modules-5.4.0-91-generic
  linux-modules-extra-5.4.0-91-generic x11-utils x11-xserver-utils xdg-utils
Use 'sudo apt autoremove' to remove them.
The following NEW packages will be installed:
  terraform
0 upgraded, 1 newly installed, 0 to remove and 105 not upgraded.
Need to get 34.9 MB of archives.
After this operation, 85.6 MB of additional disk space will be used.
Get:1 https://apt.releases.hashicorp.com focal/main amd64 terraform amd64 0.13.7 [34.9 MB]
Fetched 34.9 MB in 4min 8s (141 kB/s)
Selecting previously unselected package terraform.
(Reading database ... 201320 files and directories currently installed.)
Preparing to unpack .../terraform_0.13.7_amd64.deb ...
Unpacking terraform (0.13.7) ...
Setting up terraform (0.13.7) ...
```

- потом скачав zip архив и переименовав исполняемый файл
```
vagrant@vagrant:~$ $PATH
-bash: /home/vagrant/yandex-cloud/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin: No such file or directory
vagrant@vagrant:~$ sudo cp terraform /usr/local/sbin/terraform1.2.6
```
- результат:
```
vagrant@vagrant:~$ terraform1.2.6 -version
Terraform v1.2.6
on linux_amd64

Your version of Terraform is out of date! The latest version
is 1.3.0. You can update by downloading from https://www.terraform.io/downloads.html
vagrant@vagrant:~$ terraform -version
Terraform v0.13.7

Your version of Terraform is out of date! The latest version
is 1.3.0. You can update by downloading from https://www.terraform.io/downloads.html
---
```
### Как cдавать задание

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---
