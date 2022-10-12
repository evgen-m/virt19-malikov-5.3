# Домашнее задание к занятию "3. Введение. Экосистема. Архитектура. Жизненный цикл Docker контейнера"


Задача 1

https://hub.docker.com/repository/docker/evgenm/nginx

Задача 2

Посмотрите на сценарий ниже и ответьте на вопрос: "Подходит ли в этом сценарии использование Docker контейнеров или лучше подойдет виртуальная машина, физическая машина? Может быть возможны разные варианты?"

Детально опишите и обоснуйте свой выбор.


> Высоконагруженное монолитное java веб-приложение;

Из-за высокой нагрузки, вероятно, лучше подойдёт виртуальная машина, чтобы избежать дополнительной утилизации сети из-за инкапсуляции.

> Nodejs веб-приложение;

Подходит контейнер. Легко собирать, разворачивать и выводить для обслуживания. 

> Мобильное приложение c версиями для Android и iOS;

Для разработки не подойдёт т.к. нет UI, как серверы для размещения приложения - вероятно, Docker подойдёт.

> Шина данных на базе Apache Kafka;

Контейнер будет хорошим решением т.к. не будет той утилизации ресурсов, которые нужны на виртуализацию. 

> Elasticsearch кластер для реализации логирования продуктивного веб-приложения - три ноды elasticsearch, два logstash и две ноды kibana;

Лучше использовать виртуализацию, т.к. нет дополнительной инкапсуляции в сети (приложения высоконагруженные, логи идут постоянно), и лучше не делить ресурсы друг с другом, отдать полностью виртуальную машину под каждое приложение. 

> Мониторинг-стек на базе Prometheus и Grafana;

Лучше виртуализация, аргументы аналогично предыдущему пункту, 

> MongoDB, как основное хранилище данных для java-приложения;

В зависимости от нагрузки. Если нагрузка небольшая - можно контейнер. Если высокая нагрузка - виртуализация.

> Gitlab сервер для реализации CI/CD процессов и приватный (закрытый) Docker Registry.

Можно использовать контейнер, позволит ускорить развертывание, и как следствие, разработку.




Задача 3

> Запустите первый контейнер из образа centos c любым тэгом в фоновом режиме, подключив папку /data из текущей рабочей директории на хостовой машине в /data контейнера;

root@vagrant:/# docker run --name centos1 -v /data:/data:rw -t -d centos

> Запустите второй контейнер из образа debian в фоновом режиме, подключив папку /data из текущей рабочей директории на хостовой машине в /data контейнера;

root@vagrant:/# docker run --name debian1 -v /data:/data:rw -t -d debian


root@vagrant:/# docker ps
CONTAINER ID   IMAGE                 COMMAND                  CREATED          STATUS          PORTS                                   NAMES
cb9376fa5e19   debian                "bash"                   4 seconds ago    Up 2 seconds                                            debian1
821686b57027   centos                "/bin/bash"              34 seconds ago   Up 32 seconds                                           centos1

> Подключитесь к первому контейнеру с помощью docker exec и создайте текстовый файл любого содержания в /data;

root@vagrant:/# docker exec -it centos1 bash

[root@821686b57027 /]#

[root@821686b57027 /]# ls

bin  data  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var

[root@821686b57027 /]# cd /data

[root@821686b57027 data]# nano file_from_centos.txt

bash: nano: command not found

[root@821686b57027 data]# vi file_from_centos.txt

[root@821686b57027 data]#

[root@821686b57027 data]# cat file_from_centos.txt

Created from vm1 (centos)

[root@821686b57027 data]# exit

exit

> Добавьте еще один файл в папку /data на хостовой машине;

root@vagrant:/# cd /data

root@vagrant:/data# ls

file_from_centos.txt

root@vagrant:/data# nano file_from_host.txt

root@vagrant:/data#

root@vagrant:/data# cat file_from_host.txt

Created from host vm (ubuntu)

root@vagrant:/data#

> Подключитесь во второй контейнер и отобразите листинг и содержание файлов в /data контейнера.

root@vagrant:/data# docker exec -it debian1 bash

root@cb9376fa5e19:/# cd /data/

root@cb9376fa5e19:/data# ls

file_from_centos.txt  file_from_host.txt

root@cb9376fa5e19:/data# cat file_from_centos.txt

Created from vm1 (centos)

root@cb9376fa5e19:/data# cat file_from_host.txt

Created from host vm (ubuntu)
