## Задача 1

- Опишите своими словами основные преимущества применения на практике IaaC паттернов.

**Применение IaaC паттернов помогает сохранить конфигурацию в узлах инфраструктуры, ускорить внесение изменений, обеспечить стабильность. Избавляет от возможности дрейфа конфигураций**

- Какой из принципов IaaC является основополагающим?

**Централизованное управление конфигурациями. Инфраструктура хранится в системе контроля версий.**

## Задача 2

- Чем Ansible выгодно отличается от других систем управление конфигурациями?

**Использует существующую инфраструктуру SSH, что обеспечивает быстрый старт. Проще для понимания, чем большинство других сисем управления конфигурациями за счет декларативного метода описания.**

- Какой, на ваш взгляд, метод работы систем конфигурации более надёжный push или pull?

**Полагаю, более надёжный метод pull. Например, при недоступности целевого узла метод push не загрузит на него изменения. А при pull узел сам получит изменения, когда запустится.**

## Задача 3

Установить на личный компьютер:

- VirtualBox
- Vagrant
- Ansible

*Приложить вывод команд установленных версий каждой из программ, оформленный в markdown.*

```
andrey@andrey-comp:/media/andrey/Диск/Projects/netology$ vboxmanage --version
6.1.26_Ubuntur145957
andrey@andrey-comp:/media/andrey/Диск/Projects/netology$ vagrant --version
Vagrant 2.2.6
andrey@andrey-comp:/media/andrey/Диск/Projects/netology$ ansible --version
ansible 2.9.6
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/home/andrey/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 3.8.10 (default, Nov 26 2021, 20:14:08) [GCC 9.3.0]
```

## Задача 4 (*)

Воспроизвести практическую часть лекции самостоятельно.

- Создать виртуальную машину.
- Зайти внутрь ВМ, убедиться, что Docker установлен с помощью команды
```
docker ps
```
****
```
andrey@andrey-comp:~/netology/5.2/src/vagrant$ sudo vagrant ssh
Welcome to Ubuntu 20.04.3 LTS (GNU/Linux 5.4.0-91-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

 System information disabled due to load higher than 1.0


This system is built by the Bento project by Chef Software
More information can be found at https://github.com/chef/bento
Last login: Sat Feb  5 13:31:13 2022 from 10.0.2.2
vagrant@server1:~$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```
