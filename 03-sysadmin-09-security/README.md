# Домашнее задание к занятию "3.9. Элементы безопасности информационных систем"

1. Установите Bitwarden плагин для браузера. Зарегестрируйтесь и сохраните несколько паролей.

![1](https://raw.githubusercontent.com/Cabooki/devops-netology/main/03-sysadmin-09-security/Screenshot_250.png)
****
2. Установите Google authenticator на мобильный телефон. Настройте вход в Bitwarden акаунт через Google authenticator OTP.

![2](https://raw.githubusercontent.com/Cabooki/devops-netology/main/03-sysadmin-09-security/Screenshot_251.png)
****
3. Установите apache2, сгенерируйте самоподписанный сертификат, настройте тестовый сайт для работы по HTTPS.

**Установил Apache, добавил самоподписанный сертификат, предупреждение осталось, но в информации о сертификате есть дата окончания действия и другие данные о сертификате.**

![3](https://raw.githubusercontent.com/Cabooki/devops-netology/main/03-sysadmin-09-security/Screenshot_254.png)
****
4. Проверьте на TLS уязвимости произвольный сайт в интернете.

```
vagrant@vagrant-ubuntu-trusty-64:~/testssl.sh$ ./testssl.sh -U --sneaky https://ya.ru

###########################################################
    testssl.sh       3.1dev from https://testssl.sh/dev/
    (d720720 2021-10-03 19:52:35 -- )

      This program is free software. Distribution and
             modification under GPLv2 permitted.
      USAGE w/o ANY WARRANTY. USE IT AT YOUR OWN RISK!

       Please file bugs @ https://testssl.sh/bugs/

###########################################################

 Using "OpenSSL 1.0.2-chacha (1.0.2k-dev)" [~183 ciphers]
 on vagrant-ubuntu-trusty-64:./bin/openssl.Linux.x86_64
 (built: "Jan 18 17:12:17 2019", platform: "linux-x86_64")


 Start 2021-10-05 14:39:40        -->> 87.250.250.242:443 (ya.ru) <<--

 Further IP addresses:   2a02:6b8::2:242
 rDNS (87.250.250.242):  ya.ru.
 Service detected:       HTTP


 Testing vulnerabilities

 Heartbleed (CVE-2014-0160)                not vulnerable (OK), no heartbeat extension
 CCS (CVE-2014-0224)                       not vulnerable (OK)
 Ticketbleed (CVE-2016-9244), experiment.  not vulnerable (OK)
 ROBOT                                     not vulnerable (OK)
 Secure Renegotiation (RFC 5746)           supported (OK)
 Secure Client-Initiated Renegotiation     not vulnerable (OK)
 CRIME, TLS (CVE-2012-4929)                not vulnerable (OK)
 BREACH (CVE-2013-3587)                    potentially NOT ok, "gzip" HTTP compression detected. - only supplied "/" tested
                                           Can be ignored for static pages or if no secrets in the page
 POODLE, SSL (CVE-2014-3566)               not vulnerable (OK)
 TLS_FALLBACK_SCSV (RFC 7507)              Downgrade attack prevention supported (OK)
 SWEET32 (CVE-2016-2183, CVE-2016-6329)    VULNERABLE, uses 64 bit block ciphers
 FREAK (CVE-2015-0204)                     not vulnerable (OK)
 DROWN (CVE-2016-0800, CVE-2016-0703)      not vulnerable on this host and port (OK)
                                           make sure you don't use this certificate elsewhere with SSLv2 enabled services
                                           https://censys.io/ipv4?q=26EB381642B07A05F7CA935101FC6492F91F7F0721995A8E577EDFB6723EBD1F could help you to find out
 LOGJAM (CVE-2015-4000), experimental      not vulnerable (OK): no DH EXPORT ciphers, no DH key detected with <= TLS 1.2
 BEAST (CVE-2011-3389)                     TLS1: ECDHE-RSA-AES128-SHA AES128-SHA DES-CBC3-SHA
                                           VULNERABLE -- but also supports higher protocols  TLSv1.1 TLSv1.2 (likely mitigated)
 LUCKY13 (CVE-2013-0169), experimental     potentially VULNERABLE, uses cipher block chaining (CBC) ciphers with TLS. Check patches
 Winshock (CVE-2014-6321), experimental    not vulnerable (OK)
 RC4 (CVE-2013-2566, CVE-2015-2808)        no RC4 ciphers detected (OK)
```
****
5. Установите на Ubuntu ssh сервер, сгенерируйте новый приватный ключ. Скопируйте свой публичный ключ на другой сервер. Подключитесь к серверу по SSH-ключу.

 ```
vagrant@vagrant-ubuntu-trusty-64:~/testssl.sh$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/vagrant/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/vagrant/.ssh/id_rsa.
Your public key has been saved in /home/vagrant/.ssh/id_rsa.pub.
The key fingerprint is:
1e:5f:05:f6:91:cd:70:42:d8:58:80:12:6c:f6:15:df vagrant@vagrant-ubuntu-trusty-64cat ~/.ssh/id_rsa.pub
vagrant@vagrant-ubuntu-trusty-64:~/testssl.sh$  cat ~/.ssh/id_rsa.pub
```
**Ключ добавлен в** `/root/.ssh/authorized_keys` **целевого сервера**
```
vagrant@vagrant-ubuntu-trusty-64:~/testssl.sh$  ssh root@212.109.196.44
```
**Отключение**
```
[root@furkom ~]# exit
logout
Connection to 212.109.196.44 closed.
```
****
6. Переименуйте файлы ключей из задания 5. Настройте файл конфигурации SSH клиента, так чтобы вход на удаленный сервер осуществлялся по имени сервера.

```
vagrant@vagrant-ubuntu-trusty-64:~/testssl.sh$ sudo cat ~/.ssh/config
Host myFVDS
 HostName 212.109.196.44
 User root
 IdentityFile ~/.ssh/id_rsa
 Protocol 2
```
****
7. Соберите дамп трафика утилитой tcpdump в формате pcap, 100 пакетов. Откройте файл pcap в Wireshark.
   ![4](https://raw.githubusercontent.com/Cabooki/devops-netology/main/03-sysadmin-09-security/Screenshot_255.png)