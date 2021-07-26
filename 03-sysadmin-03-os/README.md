# Домашнее задание к занятию "3.3. Операционные системы, лекция 1"

1. Какой системный вызов делает команда `cd`? В прошлом ДЗ мы выяснили, что `cd` не является самостоятельной  программой, это `shell builtin`, поэтому запустить `strace` непосредственно на `cd` не получится. Тем не менее, вы можете запустить `strace` на `/bin/bash -c 'cd /tmp'`. В этом случае вы увидите полный список системных вызовов, которые делает сам `bash` при старте. Вам нужно найти тот единственный, который относится именно к `cd`.

**Полагаю подразумевается вызов `chdir("/tmp")` полностью строка - `chdir("/tmp")                           = 0`**

2. Попробуйте использовать команду `file` на объекты разных типов на файловой системе. Например:
    ```bash
    vagrant@netology1:~$ file /dev/tty
    /dev/tty: character special (5/0)
    vagrant@netology1:~$ file /dev/sda
    /dev/sda: block special (8/0)
    vagrant@netology1:~$ file /bin/bash
    /bin/bash: ELF 64-bit LSB shared object, x86-64
    ```
   Используя `strace` выясните, где находится база данных `file` на основании которой она делает свои догадки.

**Судя по всему `file` получает данные из `/usr/lib/x86_64-linux-gnu/gconv/gconv-modules.cache`**
****
**Исправление**

```
stat("/home/vagrant/.magic.mgc", 0x7ffd27aba320) = -1 ENOENT (No such file or directory)
stat("/home/vagrant/.magic", 0x7ffd27aba320) = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/magic.mgc", O_RDONLY) = -1 ENOENT (No such file or directory)
stat("/etc/magic", {st_mode=S_IFREG|0644, st_size=111, ...}) = 0
openat(AT_FDCWD, "/etc/magic", O_RDONLY) = 3
```
****
3. Предположим, приложение пишет лог в текстовый файл. Этот файл оказался удален (deleted в lsof), однако возможности сигналом сказать приложению переоткрыть файлы или просто перезапустить приложение – нет. Так как приложение продолжает писать в удаленный файл, место на диске постепенно заканчивается. Основываясь на знаниях о перенаправлении потоков предложите способ обнуления открытого удаленного файла (чтобы освободить место на файловой системе).

**Изменение в последней строке.**

```angular2html
vagrant@vagrant:~$ touch /tmp/newfile
vagrant@vagrant:~$ vim /tmp/newfile
vagrant@vagrant:~$ cat /tmp/newfile
sometext
vagrant@vagrant:~$ lsof -p 1691 | grep newfile
python3 1691 vagrant    3r   REG  253,0        9 2240933 /tmp/newfile
vagrant@vagrant:~$ rm /tmp/newfile
vagrant@vagrant:~$ cat /tmp/newfile
cat: /tmp/newfile: No such file or directory
vagrant@vagrant:~$ lsof -p 1691 | grep newfile
python3 1691 vagrant    3r   REG  253,0        9 2240933 /tmp/newfile (deleted)
vagrant@vagrant:~$ cat /proc/1691/fd/3
sometext
# vagrant@vagrant:~$ cat /proc/1691/fd/3 > /tmp/newfile
> /proc/1691/fd/3
```

4. Занимают ли зомби-процессы какие-то ресурсы в ОС (CPU, RAM, IO)?

**Занимают только ROM, так как это файлы.**
****
**Исправление 1**

**"Вы пишете, что “занимают только ROM”. Что вы понимаете под этим? Read-Only Memory?""**

**Да, имел в виду её. Но так как файлы пустые, то и эту память они не занимают.**

**Исправление 2**

**Только лимит записей kernel.pid_max**

****
5. В iovisor BCC есть утилита `opensnoop`:
    ```bash
    root@vagrant:~# dpkg -L bpfcc-tools | grep sbin/opensnoop
    /usr/sbin/opensnoop-bpfcc
    ```
   На какие файлы вы увидели вызовы группы `open` за первую секунду работы утилиты? Воспользуйтесь пакетом `bpfcc-tools` для Ubuntu 20.04. Дополнительные [сведения по установке](https://github.com/iovisor/bcc/blob/master/INSTALL.md).

**Если правильно понял, то имеется в виду этот вывод.**

```
vagrant@vagrant:~$ sudo opensnoop-bpfcc
PID    COMM               FD ERR PATH
873    vminfo              4   0 /var/run/utmp
668    dbus-daemon        -1   2 /usr/local/share/dbus-1/system-services
668    dbus-daemon        18   0 /usr/share/dbus-1/system-services
668    dbus-daemon        -1   2 /lib/dbus-1/system-services
668    dbus-daemon        18   0 /var/lib/snapd/dbus-1/system-services/
1      systemd            12   0 /proc/438/cgroup
684    irqbalance          6   0 /proc/interrupts
684    irqbalance          6   0 /proc/stat
684    irqbalance          6   0 /proc/irq/20/smp_affinity
684    irqbalance          6   0 /proc/irq/0/smp_affinity
684    irqbalance          6   0 /proc/irq/1/smp_affinity
684    irqbalance          6   0 /proc/irq/8/smp_affinity
684    irqbalance          6   0 /proc/irq/12/smp_affinity
684    irqbalance          6   0 /proc/irq/14/smp_affinity
684    irqbalance          6   0 /proc/irq/15/smp_affinity
873    vminfo              4   0 /var/run/utmp
668    dbus-daemon        -1   2 /usr/local/share/dbus-1/system-services
668    dbus-daemon        18   0 /usr/share/dbus-1/system-services
668    dbus-daemon        -1   2 /lib/dbus-1/system-services
668    dbus-daemon        18   0 /var/lib/snapd/dbus-1/system-services/
```


6. Какой системный вызов использует `uname -a`? Приведите цитату из man по этому системному вызову, где описывается альтернативное местоположение в `/proc`, где можно узнать версию ядра и релиз ОС.

**`uname -a` использует `execve("/usr/bin/uname", ["uname", "-a"], 0x5590af838650 /* 24 vars */) = 0"`**

**Не совсем понял... `man uname` выдает всего 35 строк, где нет ничего о системном вызове.**
****
**Исправление**

**sysinfo({uptime=3216, loads=[1344, 1696, 160], totalram=2084085760, freeram=1384054784, sharedram=700416, bufferram=47779840, totalswap=1027600384, freeswap=1027600384, procs=129, totalhigh=0, freehigh=0, mem_unit=1}) = 0**

**Part of the utsname information is also accessible via
/proc/sys/kernel/{ostype, hostname, osrelease, version,
domainname}.**
****
7. Чем отличается последовательность команд через `;` и через `&&` в bash? Например:
    ```bash
    root@netology1:~# test -d /tmp/some_dir; echo Hi
    Hi
    root@netology1:~# test -d /tmp/some_dir && echo Hi
    root@netology1:~#
    ```
   Есть ли смысл использовать в bash `&&`, если применить `set -e`?

**`;` используется как разделитель, команды выполняются независимо от результата.**

**При использовании `&&` команда справа отработает только если команда слева завершится с результатом `0`**

8. Из каких опций состоит режим bash `set -euxo pipefail` и почему его хорошо было бы использовать в сценариях?
```
-e  Exit immediately if a command exits with a non-zero status.
-u  Treat unset variables as an error when substituting.
-x  Print commands and their arguments as they are executed.
-o option-name
          Set the variable corresponding to option-name:
          
          pipefail     the return value of a pipeline is the status of
                       the last command to exit with a non-zero status,
                       or zero if no command exited with a non-zero status
```

**Насколько понимаю, использование данного режима в сценариях позволяет отследить команды, завершившиеся с ненулевым результатом.**

9. Используя `-o stat` для `ps`, определите, какой наиболее часто встречающийся статус у процессов в системе. В `man ps` ознакомьтесь (`/PROCESS STATE CODES`) что значат дополнительные к основной заглавной буквы статуса процессов. Его можно не учитывать при расчете (считать S, Ss или Ssl равнозначными).

**Наиболее часто встречающийся статус `S - interruptible sleep (waiting for an event to complete)`**

**В `man` строка 347**