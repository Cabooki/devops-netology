# Домашнее задание к занятию "4.2. Использование Python для решения типовых DevOps задач"

## Обязательные задания

1. Есть скрипт:
   ```python
   #!/usr/bin/env python3
   a = 1
   b = '2'
   c = a + b
   ```
	* Какое значение будет присвоено переменной c?

	  **Переменной c не будет присвоено значения, так как в операции использованы переменный разных типов (a - int, b - str)**
	* Как получить для переменной c значение 12?
   ```python
	  >>> c = str(a) + b
	  >>> c
	  '12'
   ```

	* Как получить для переменной c значение 3?

   ```python
   >>> c = a + int(b)
   >>> c
   3
    ```
****
2. Мы устроились на работу в компанию, где раньше уже был DevOps Engineer. Он написал скрипт, позволяющий узнать, какие файлы модифицированы в репозитории, относительно локальных изменений. Этим скриптом недовольно начальство, потому что в его выводе есть не все изменённые файлы, а также непонятен полный путь к директории, где они находятся. Как можно доработать скрипт ниже, чтобы он исполнял требования вашего руководителя?

   ```python
   #!/usr/bin/env python3

   import os

   bash_command = ["cd ~/netology/sysadm-homeworks", "git status"]
   result_os = os.popen(' && '.join(bash_command)).read()
   is_change = False
   for result in result_os.split('\n'):
       if result.find('modified') != -1:
           prepare_result = result.replace('\tmodified:   ', '')
           print(prepare_result)
           break

   ```

   **Ответ**

   ```python
   #!/usr/bin/env python3

   import os

   bash_command = ["cd ~/homework/devops-netology", "git status"]
   result_os = os.popen(' && '.join(bash_command)).read()
   is_change = False
   for result in result_os.split('\n'):
     if result.find('modified') != -1:
       prepare_result = os.path.abspath("~/homework/devops-netology/" + result.replace('\tmodified:   ', ''))
       print(prepare_result)
    ```
****
3. Доработать скрипт выше так, чтобы он мог проверять не только локальный репозиторий в текущей директории, а также умел воспринимать путь к репозиторию, который мы передаём как входной параметр. Мы точно знаем, что начальство коварное и будет проверять работу этого скрипта в директориях, которые не являются локальными репозиториями.

   ```python
   #!/usr/bin/env python3
   
   import os
   import sys

   # /netology/sysadm-homeworks
   if len(sys.argv) > 1:
     directory = sys.argv[1]
   else:
     directory = '~/homework/devops-netology'
   bash_command = ["cd " + directory, "git status"]
   result_os = os.popen(' && '.join(bash_command)).read()
   is_change = False
   for result in result_os.split('\n'):
     if result.find('modified') != -1:
       prepare_result = os.path.abspath(sys.argv[1] + '/' + result.replace('\tmodified:   ', ''))
       print(prepare_result)
   ```
****
4. Наша команда разрабатывает несколько веб-сервисов, доступных по http. Мы точно знаем, что на их стенде нет никакой балансировки, кластеризации, за DNS прячется конкретный IP сервера, где установлен сервис. Проблема в том, что отдел, занимающийся нашей инфраструктурой очень часто меняет нам сервера, поэтому IP меняются примерно раз в неделю, при этом сервисы сохраняют за собой DNS имена. Это бы совсем никого не беспокоило, если бы несколько раз сервера не уезжали в такой сегмент сети нашей компании, который недоступен для разработчиков. Мы хотим написать скрипт, который опрашивает веб-сервисы, получает их IP, выводит информацию в стандартный вывод в виде: <URL сервиса> - <его IP>. Также, должна быть реализована возможность проверки текущего IP сервиса c его IP из предыдущей проверки. Если проверка будет провалена - оповестить об этом в стандартный вывод сообщением: [ERROR] <URL сервиса> IP mismatch: <старый IP> <Новый IP>. Будем считать, что наша разработка реализовала сервисы: drive.google.com, mail.google.com, google.com.

   ```python
   #!/usr/bin/env python3.7
   
   import subprocess
   import socket
   
   domains = ['mail.google.com', 'drive.google.com', 'google.com']
   domains_file = open('domains.txt', 'r')
   IPdomains = {}
   old_domains_res = {}
   
   while True:
     domain_with_ip = domains_file.readline()
     if not domain_with_ip:
       break
     old_domain_tmp = domain_with_ip.split(':')
     old_domains_res[old_domain_tmp[0]] = old_domain_tmp[1].replace('\n', '')
   
   domains_file.close()
   domains_file = open('domains.txt', 'w')
   
   for domain in domains:
     IPdomains[domain] = socket.gethostbyname(domain)
     domains_file.write(domain + ':' + IPdomains[domain] + '\n')
     if len(old_domains_res) == 3:
       if socket.gethostbyname(domain) == old_domains_res[domain]:
           print(domain + ' - ' + socket.gethostbyname(domain))
       else:
          print('[ERROR] ' + domain + ' IP mismatch: ' + socket.gethostbyname(domain) + ' ' + old_domains_res[domain])
       else:
         print('First run!')
   ```