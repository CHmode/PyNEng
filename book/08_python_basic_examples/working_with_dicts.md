## Работа со словарями

При обработке вывода команд или конфигурации часто надо будет записать итоговые данные в словарь.

Не всегда очевидно как обрабатывать вывод команд и каким образом в целом подходить к разбору вывода на части.
В этом подразделе рассматриваются несколько примеров, с возрастающим уровнем сложности.

### Разбор вывода столбцами

В этом примере будет разбираться вывод команды sh ip int br.
Из вывода команды нам надо получить соответствия имя интерфейса - IP-адрес.
То есть имя интерфейса - это ключ словаря, а IP-адрес - значение.
При этом, соответствие надо делать только для тех интерфейсов, у которых назначен IP-адрес.

Пример вывода команды sh ip int br (файл sh_ip_int_br.txt):
```
R1#show ip interface brief
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0/0            15.0.15.1       YES manual up                    up
FastEthernet0/1            10.0.12.1       YES manual up                    up
FastEthernet0/2            10.0.13.1       YES manual up                    up
FastEthernet0/3            unassigned      YES unset  up                    down
Loopback0                  10.1.1.1        YES manual up                    up
Loopback100                100.0.0.1       YES manual up                    up
```


Команда sh ip int br отображает вывод столбцами.
Значит нужные поля находятся в одной строке.

Файл working_with_dict_example_1.py:
```python
result = {}

with open('sh_ip_int_br.txt') as f:
    for line in f:
        line = line.split()
        if line and line[1][0].isdigit():
            interface, address, *other = line
            result[interface] = address

print(result)
```

