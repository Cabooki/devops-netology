
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

**Не знаю, правильно ли я понял, но сделал, как понял. Ссылка на образ с тегом  [nginx](https://hub.docker.com/layers/190943480/cabooki/netology/nginx/images/sha256-fa382679c12e4ea215bf784d386248d7a7b3b358295ea6494f00d151bf31a45e?context=repo). Ссылка на [репозиторий](https://hub.docker.com/repository/docker/cabooki/netology).**

## Задача 2

Посмотрите на сценарий ниже и ответьте на вопрос:
"Подходит ли в этом сценарии использование Docker контейнеров или лучше подойдет виртуальная машина, физическая машина? Может быть возможны разные варианты?"

Детально опишите и обоснуйте свой выбор.

--

Сценарий:

- Высоконагруженное монолитное java веб-приложение;

*Физическая либо виртуальная машина*

- Nodejs веб-приложение;

*Контейнер Docker*

- Мобильное приложение c версиями для Android и iOS;

*Контейнер Docker*

- Шина данных на базе Apache Kafka;

*Виртуальная машина*

- Elasticsearch кластер для реализации логирования продуктивного веб-приложения - три ноды elasticsearch, два logstash и две ноды kibana;

*Виртуальная машина*

- Мониторинг-стек на базе Prometheus и Grafana;

*Контейнер Docker*

- MongoDB, как основное хранилище данных для java-приложения;

*Физическая машина*

- Gitlab сервер для реализации CI/CD процессов и приватный (закрытый) Docker Registry.

*Контейнер Docker*

## Задача 3

- Запустите первый контейнер из образа ***centos*** c любым тэгом в фоновом режиме, подключив папку ```/data``` из текущей рабочей директории на хостовой машине в ```/data``` контейнера;
- Запустите второй контейнер из образа ***debian*** в фоновом режиме, подключив папку ```/data``` из текущей рабочей директории на хостовой машине в ```/data``` контейнера;
- Подключитесь к первому контейнеру с помощью ```docker exec``` и создайте текстовый файл любого содержания в ```/data```;
- Добавьте еще один файл в папку ```/data``` на хостовой машине;
- Подключитесь во второй контейнер и отобразите листинг и содержание файлов в ```/data``` контейнера.

```
root@vagrant:~# docker pull centos
root@vagrant:~# docker run -t -d --name centos-1 -v /root/data:/share/data centos
root@vagrant:~# docker pull debian
root@vagrant:~# docker run -t -d --name debian-2 -v /root/data:/data debian:latest
root@vagrant:~# docker exec -it centos-1 bash
[root@d7c8cf2659ef /]# echo 'First text' > /share/data/first.txt
[root@d7c8cf2659ef /]# exit
exit
root@vagrant:~# echo 'Second text' > data/second.txt
root@vagrant:~# docker exec -it debian-2 bash
root@2fc93d475017:/# ls data
first.txt  second.txt
root@2fc93d475017:/# cat data/first.txt data/second.txt
First text
Second text
```

## Задача 4 (*)

Воспроизвести практическую часть лекции самостоятельно.

Соберите Docker образ с Ansible, загрузите на Docker Hub и пришлите ссылку вместе с остальными ответами к задачам.
****
Ссылка на образ с тегом **[Ansible](https://hub.docker.com/layers/190943848/cabooki/netology/ansible/images/sha256-f347d677e9caec7548324ad35ca8c7c5d66b9711b3d3c5729b88a4eadb435a7f?context=repo)**
