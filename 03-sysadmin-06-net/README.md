# Домашнее задание к занятию "3.6. Компьютерные сети, лекция 1"

1. Работа c HTTP через телнет.
- Подключитесь утилитой телнет к сайту stackoverflow.com
`telnet stackoverflow.com 80`
- отправьте HTTP запрос
```bash
GET /questions HTTP/1.0
HOST: stackoverflow.com
[press enter]
[press enter]
```
- В ответе укажите полученный HTTP код, что он означает?

```
vagrant@vagrant:~$ telnet stackoverflow.com 80
Trying 151.101.1.69...
Connected to stackoverflow.com.
Escape character is '^]'.
GET /questions HTTP/1.0
HOST: stackoverflow.com

HTTP/1.1 301 Moved Permanently
cache-control: no-cache, no-store, must-revalidate
location: https://stackoverflow.com/questions
x-request-guid: 40039463-58b4-4e16-9052-f79ff11af15c
feature-policy: microphone 'none'; speaker 'none'
content-security-policy: upgrade-insecure-requests; frame-ancestors 'self' https://stackexchange.com
Accept-Ranges: bytes
Date: Sat, 02 Oct 2021 22:19:21 GMT
Via: 1.1 varnish
Connection: close
X-Served-By: cache-hel6822-HEL
X-Cache: MISS
X-Cache-Hits: 0
X-Timer: S1633213161.338845,VS0,VE102
Vary: Fastly-SSL
X-DNS-Prefetch-Control: off
Set-Cookie: prov=f1cfde46-4715-fee1-aed5-ad45fbb5d55e; domain=.stackoverflow.com; expires=Fri, 01-Jan-2055 00:00:00 GMT; path=/; HttpOnly

Connection closed by foreign host.
```
**Означает перенаправление с запрашиваемой страницы.**

2. Повторите задание 1 в браузере используя консоль разработчика F12.
- откройте вкладку `Network`
- отправте запрос http://stackoverflow.com
- найдите первый ответ HTTP сервера, откройте вкладку `Headers`
- Укажите в ответе полученный HTTP код.
- Проверьте время загрузки страницы, какой запрос обрабатывался дольше всего?
- Приложите скриншот консоли браузера в ответ.

**Первый ответ - редирект с http на https. Время загрузки страницы и самый долгий запрос на скриншоте.**

![1](https://github.com/Cabooki/devops-netology/tree/main/03-sysadmin-03-os/03-sysadmin-06-net/Screenshot_246.png)
![2](https://github.com/Cabooki/devops-netology/tree/main/03-sysadmin-03-os/03-sysadmin-06-net/Screenshot_247.png)

3. Какой IP адрес у вас в интернете? воспользуйтесь сайтом whoer.net.
4. Какому провайдеру принадлежит ваш IP адрес? Какой автономной системе AS? воспользуйтесь утилитой `whois`.
5. Через какие сети проходит пакет отправленный с вашего компьютера на адрес 8.8.8.8? Через какие AS? воспользуйтесь утилитой `traceroute`
6. Повторите задание 5 в утилите `mtr`. На каком участке наибольшая задержка - delay?
7. Каких DNS сервера отвечают за доменное имя dns.google? Какие A записи? воспользуйтесь `dig +trace`
8. Проверьте PTR записи для IP адресов из задания 7. Какое доменное имя привязано к IP? воспользуйтесь `dig -x`

В качестве ответов на вопросы можно приложите лог выполнения команд в консоли или скриншот полученных результатов.

 
 ---

## Как сдавать задания

Обязательными к выполнению являются задачи без указания звездочки. Их выполнение необходимо для получения зачета и диплома о профессиональной переподготовке.

Задачи со звездочкой (*) являются дополнительными задачами и/или задачами повышенной сложности. Они не являются обязательными к выполнению, но помогут вам глубже понять тему.

Домашнее задание выполните в файле readme.md в github репозитории. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.

Также вы можете выполнить задание в [Google Docs](https://docs.google.com/document/u/0/?tgif=d) и отправить в личном кабинете на проверку ссылку на ваш документ.
Название файла Google Docs должно содержать номер лекции и фамилию студента. Пример названия: "1.1. Введение в DevOps — Сусанна Алиева".

Если необходимо прикрепить дополнительные ссылки, просто добавьте их в свой Google Docs.

Перед тем как выслать ссылку, убедитесь, что ее содержимое не является приватным (открыто на комментирование всем, у кого есть ссылка), иначе преподаватель не сможет проверить работу. Чтобы это проверить, откройте ссылку в браузере в режиме инкогнито.

[Как предоставить доступ к файлам и папкам на Google Диске](https://support.google.com/docs/answer/2494822?hl=ru&co=GENIE.Platform%3DDesktop)

[Как запустить chrome в режиме инкогнито ](https://support.google.com/chrome/answer/95464?co=GENIE.Platform%3DDesktop&hl=ru)

[Как запустить  Safari в режиме инкогнито ](https://support.apple.com/ru-ru/guide/safari/ibrw1069/mac)

Любые вопросы по решению задач задавайте в чате Slack.

---
