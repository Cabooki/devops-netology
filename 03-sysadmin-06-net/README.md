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
****
2. Повторите задание 1 в браузере используя консоль разработчика F12.
- откройте вкладку `Network`
- отправте запрос http://stackoverflow.com
- найдите первый ответ HTTP сервера, откройте вкладку `Headers`
- Укажите в ответе полученный HTTP код.
- Проверьте время загрузки страницы, какой запрос обрабатывался дольше всего?
- Приложите скриншот консоли браузера в ответ.

**Первый ответ - редирект с http на https. Время загрузки страницы и самый долгий запрос на скриншоте.**

![1](https://raw.githubusercontent.com/Cabooki/devops-netology/main/03-sysadmin-06-net/Screenshot_246.png)
![2](https://raw.githubusercontent.com/Cabooki/devops-netology/main/03-sysadmin-06-net/Screenshot_247.png)
****
3. Какой IP адрес у вас в интернете? воспользуйтесь сайтом whoer.net.

![2](https://raw.githubusercontent.com/Cabooki/devops-netology/main/03-sysadmin-06-net/Screenshot_248.png)
****
4. Какому провайдеру принадлежит ваш IP адрес? Какой автономной системе AS? воспользуйтесь утилитой `whois`.

```
vagrant@vagrant:~$ whois 94.41.85.255
descr:          JSC "Ufanet", Ufa, Russia
origin:         AS24955
```
****
5. Через какие сети проходит пакет отправленный с вашего компьютера на адрес 8.8.8.8? Через какие AS? воспользуйтесь утилитой `traceroute`

`traceroute` не показывает информации о сетях для 8.8.8.8

```
vagrant@vagrant:~$ traceroute -An 8.8.8.8
traceroute to 8.8.8.8 (8.8.8.8), 30 hops max, 60 byte packets
 1  10.0.2.2 [*]  0.659 ms  0.609 ms  0.588 ms
 2  * * *
 3  * * *
 4  * * *
 5  * * *
 6  * * *
 7  * * *
 8  * * *
 9  * * *
10  * * *
11  * * *
12  * * *
13  * * *
14  * * *
15  * * *
16  * * *
17  * * *
18  * * *
19  * * *
20  * * *
21  * * *
22  * * *
23  * * *
24  * * *
25  * * *
26  * * *
27  * * *
28  * * *
29  * * *
30  * * *
```
****
6. Повторите задание 5 в утилите `mtr`. На каком участке наибольшая задержка - delay?

```
vagrant (10.0.2.15)                                                                            2021-10-02T23:01:51+0000
Keys:  Help   Display mode   Restart statistics   Order of fields   quit
                                                                               Packets               Pings
 Host                                                                        Loss%   Snt   Last   Avg  Best  Wrst StDev
 1. AS???    10.0.2.2                                                         0.0%     9    0.3   0.6   0.3   1.1   0.3
 2. AS???    192.168.0.1                                                      0.0%     9    3.5   4.3   3.0   5.4   0.9
 3. AS24955  92.50.191.11                                                     0.0%     9    8.8   7.3   4.7   9.6   1.7
 4. AS24955  92.50.191.62                                                     0.0%     9    2.8   6.2   2.8   9.6   1.7
 5. AS???    10.2.3.33                                                        0.0%     9    4.9   7.2   4.9  10.0   1.5
 6. AS???    10.1.190.62                                                      0.0%     9    6.4   7.2   6.4   8.9   0.8
 7. AS???    10.3.2.3                                                         0.0%     9   28.0  29.0  26.1  38.5   3.7
 8. AS???    10.3.2.34                                                        0.0%     9   21.7  23.0  21.1  25.6   1.6
 9. AS47775  213.5.104.27                                                     0.0%     9   23.1  24.7  22.6  30.1   2.3
10. AS15169  172.253.77.173                                                   0.0%     8   22.4  23.2  21.0  25.2   1.5
11. AS15169  108.170.250.83                                                   0.0%     8   26.9  29.5  25.7  41.5   5.1
12. AS15169  142.250.239.64                                                  37.5%     8   40.0  44.1  39.1  60.7   9.3
13. AS15169  216.239.48.224                                                   0.0%     8   40.4  45.6  38.0  89.9  17.9
14. AS15169  209.85.246.111                                                   0.0%     8   38.5  39.4  38.5  40.9   0.9
15. (waiting for reply)
16. (waiting for reply)
17. (waiting for reply)
18. (waiting for reply)
19. (waiting for reply)
20. (waiting for reply)
21. (waiting for reply)
22. (waiting for reply)
23. (waiting for reply)
24. AS15169  8.8.8.8                                                          0.0%     8   40.4  38.5  37.2  40.8   1.4
```
**Наибольшая задержка в сети** `13. AS15169  216.239.48.224 `

****
7. Каких DNS сервера отвечают за доменное имя dns.google? Какие A записи? воспользуйтесь `dig +trace`

```
vagrant@vagrant:~$ dig +trace dns.google | grep dns
; <<>> DiG 9.16.1-Ubuntu <<>> +trace dns.google
dns.google.             10800   IN      NS      ns1.zdns.google.
dns.google.             10800   IN      NS      ns2.zdns.google.
dns.google.             10800   IN      NS      ns4.zdns.google.
dns.google.             10800   IN      NS      ns3.zdns.google.
dns.google.             3600    IN      DS      56044 8 2 1B0A7E90AA6B1AC65AA5B573EFC44ABF6CB2559444251B997103D2E4 0C351B08
dns.google.             3600    IN      RRSIG   DS 8 2 3600 20211021180238 20210929180238 2671 google. cC+IGAuFCtazvsX1o73BOVcp5JThpZoXPJvRVh4JP4N9Qw40yATbS78U OkeTVkH2ETbh7LvGD8gKcoSE9o5TSd0EPYsVUnk9Rn4oBFJ6SqtVhA2D /rXZg3im/1TJJ61yo3j1Y0qwd8ebDOw49rH8SpDcTf1fChxX0UJ28U1B s/s=
dns.google.             900     IN      A       8.8.8.8
dns.google.             900     IN      A       8.8.4.4
dns.google.             900     IN      RRSIG   A 8 2 900 20211101160907 20211002160907 1773 dns.google. F2lLMXqqU/HkZOb2luNu/U/sYEHURkR+2KQGzkig4BcAm3lcSX5VfjND XpJ2JWE9Opecj/HBmxjFBtIbgXXdI19NY2fJieXzQFShnmUKwM9FDs5h 37N16xJeOx5F26aZDKcnTRvZ8KwG2ioJBGZ/Bp6SJOK7S9ji5d8rT3KT 9gE=
;; Received 241 bytes from 216.239.36.114#53(ns3.zdns.google) in 47 ms
```
****
8. Проверьте PTR записи для IP адресов из задания 7. Какое доменное имя привязано к IP? воспользуйтесь `dig -x`

```
vagrant@vagrant:~$ dig -x 8.8.8.8
;; ANSWER SECTION:
8.8.8.8.in-addr.arpa.   1692    IN      PTR     dns.google.
```