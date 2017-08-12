## Модуль telnetlib

Модуль telnetlib входит в стандартную библиотеку Python. Это реализация клиента telnet.

> Подключиться по telnet можно и используя pexpect. Плюс telnetlib в том, что этот модуль входит в стандартную библиотеку Python.

Принцип работы telnetlib напоминает pexpect, поэтому пример ниже должен быть понятен.

Файл 2_telnetlib.py:
```python
import telnetlib
import time
import getpass
import sys

COMMAND = sys.argv[1].encode('utf-8')
USER = input("Username: ").encode('utf-8')
PASSWORD = getpass.getpass().encode('utf-8')
ENABLE_PASS = getpass.getpass(prompt='Enter enable password: ').encode('utf-8')

DEVICES_IP = ['192.168.100.1','192.168.100.2','192.168.100.3']

for IP in DEVICES_IP:
    print("Connection to device {}".format(IP))
    with telnetlib.Telnet(IP) as t:

        t.read_until(b"Username:")
        t.write(USER + b'\n')

        t.read_until(b"Password:")
        t.write(PASSWORD + b'\n')
        t.write(b"enable\n")

        t.read_until(b"Password:")
        t.write(ENABLE_PASS + b'\n')
        t.write(b"terminal length 0\n")
        t.write(COMMAND + b'\n')

        time.sleep(5)

        output = t.read_very_eager().decode('utf-8')
        print(output)

```

Первая особенность, которая бросается в глаза - в конце отправляемых команд, надо добавлять символ перевода строки.

> Использование объекта Telnet как менеджера контекса добавлено в версии 3.6

В остальном, telnetlib очень похож на pexpect:
* ```with telnetlib.Telnet(ip) as t``` - класс Telnet представляет соединение к серверу.
 * в данном случае, ему передается только IP-адрес, но можно передать и порт, к которому нужно подключаться
* ```read_until``` - похож на метод ```expect``` в модуле pexpect. Указывает до какой строки следует считывать вывод
* ```write``` - передать строку
* ```read_very_eager``` - считать всё, что получается


Выполнение скрипта:
```
$ python 2_telnetlib.py "sh ip int br"
Username: cisco
Password:
Enter enable secret:
Connection to device 192.168.100.1

R1#terminal length 0
R1#sh ip int br
Interface              IP-Address      OK? Method Status                Protocol
FastEthernet0/0        192.168.100.1   YES NVRAM  up                    up
FastEthernet0/1        unassigned      YES NVRAM  up                    up
FastEthernet0/1.10     10.1.10.1       YES manual up                    up
FastEthernet0/1.20     10.1.20.1       YES manual up                    up
FastEthernet0/1.30     10.1.30.1       YES manual up                    up
FastEthernet0/1.40     10.1.40.1       YES manual up                    up
FastEthernet0/1.50     10.1.50.1       YES manual up                    up
FastEthernet0/1.60     10.1.60.1       YES manual up                    up
FastEthernet0/1.70     10.1.70.1       YES manual up                    up
R1#
Connection to device 192.168.100.2

R2#terminal length 0
R2#sh ip int br
Interface              IP-Address      OK? Method Status                Protocol
FastEthernet0/0        192.168.100.2   YES NVRAM  up                    up
FastEthernet0/1        unassigned      YES NVRAM  up                    up
FastEthernet0/1.10     10.2.10.1       YES manual up                    up
FastEthernet0/1.20     10.2.20.1       YES manual up                    up
FastEthernet0/1.30     10.2.30.1       YES manual up                    up
FastEthernet0/1.40     10.2.40.1       YES manual up                    up
FastEthernet0/1.50     10.2.50.1       YES manual up                    up
FastEthernet0/1.60     10.2.60.1       YES manual up                    up
FastEthernet0/1.70     10.2.70.1       YES manual up                    up
R2#
Connection to device 192.168.100.3

R3#terminal length 0
R3#sh ip int br
Interface              IP-Address      OK? Method Status                Protocol
FastEthernet0/0        192.168.100.3   YES NVRAM  up                    up
FastEthernet0/1        unassigned      YES NVRAM  up                    up
FastEthernet0/1.10     10.3.10.1       YES manual up                    up
FastEthernet0/1.20     10.3.20.1       YES manual up                    up
FastEthernet0/1.30     10.3.30.1       YES manual up                    up
FastEthernet0/1.40     10.3.40.1       YES manual up                    up
FastEthernet0/1.50     10.3.50.1       YES manual up                    up
FastEthernet0/1.60     10.3.60.1       YES manual up                    up
FastEthernet0/1.70     10.3.70.1       YES manual up                    up
R3#
```

Мы не будем подробнее останавливаться на telnetlib. Остальные методы, которые поддерживает модуль, можно посмотреть в документации.


