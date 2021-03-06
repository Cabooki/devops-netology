# Домашнее задание к занятию "4.1. Командная оболочка Bash: Практические навыки"

1. Есть скрипт:
	```bash
	a=1
	b=2
	c=a+b
	d=$a+$b
	e=$(($a+$b))
	```
	* Какие значения переменным c,d,e будут присвоены?
	* Почему?

```bash
root@vagrant-ubuntu-trusty-64:~# a=1
root@vagrant-ubuntu-trusty-64:~# b=2
root@vagrant-ubuntu-trusty-64:~# c=a+b
root@vagrant-ubuntu-trusty-64:~# echo $c
a+b
root@vagrant-ubuntu-trusty-64:~# d=$a+$b
root@vagrant-ubuntu-trusty-64:~# echo $d
1+2
root@vagrant-ubuntu-trusty-64:~# e=$(($a+$b))
root@vagrant-ubuntu-trusty-64:~# echo $e
3
```
**`c=a+b` - в переменную `c` записывается строка**

**`d=$a+$b` - в переменную `d` записывается строка, где вместо `$a` и `$b` подставляются значения переменных `a` и `b` соответсвенно.**

**`e=$(($a+$b))` - в двойных круглых скобках происходит сложение значений переменных, полученное значение записывается в переменную `e`**
****
2. На нашем локальном сервере упал сервис и мы написали скрипт, который постоянно проверяет его доступность, записывая дату проверок до тех пор, пока сервис не станет доступным. В скрипте допущена ошибка, из-за которой выполнение не может завершиться, при этом место на Жёстком Диске постоянно уменьшается. Что необходимо сделать, чтобы его исправить:
	```bash
	while ((1==1))
	do
	curl https://localhost:4757
	if (($? != 0))
	then
	date >> curl.log
	fi
	done
	```
**В текущем виде скрипт работать не может, так как в строке `while ((1==1)` не хватает второй закрывающей скобки. После добавления скрипт работает бесконечно.**

**Скрипт не остановится даже если статус ответа придёт `0`, так как нет завершающей команды `break`.**

**В условном операторе**
```bash
	if (($? != 0))
	then
	date >> curl.log
	fi
```
**следует добавить завершение при нулевом ответе**
```bash
	if (($? != 0))
	then
	date >> curl.log
	else
	break
	fi
```
****
3. Необходимо написать скрипт, который проверяет доступность трёх IP: 192.168.0.1, 173.194.222.113, 87.250.250.242 по 80 порту и записывает результат в файл log. Проверять доступность необходимо пять раз для каждого узла.

```bash
arr_ips=(192.168.0.1 173.194.222.113 87.250.250.242)
for i in ${!arr_ips[@]}
do
j=5
while (($j > 0))
do
curl http://${arr_ips[$i]} > /dev/null 2>&1
echo $? >> log
echo ${arr_ips[$i]} >> log
let "j -= 1"
done
done
```
****
4. Необходимо дописать скрипт из предыдущего задания так, чтобы он выполнялся до тех пор, пока один из узлов не окажется недоступным. Если любой из узлов недоступен - IP этого узла пишется в файл error, скрипт прерывается

```bash
arr_ips=(192.168.0.1 173.194.222.113 87.250.250.242)
for i in ${!arr_ips[@]}
do
curl http://${arr_ips[$i]} > /dev/null 2>&1
if (($? != 0))
then
echo ${arr_ips[$i]} >> error
break
fi
done
```