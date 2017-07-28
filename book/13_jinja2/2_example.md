{% raw %}
## Пример использования Jinja с корректным использованием программного интерфейса

Для того чтобы разобраться с Jinja2, лучше использовать предыдущие примеры. В этом разделе описывается корректное использование Jinja. В таком варианте данные, шаблон и скрипт, который генерирует итоговую информацию, разделены.

> Термин "программный интерфейс" относится к способу работы Jinja с вводными данными и шаблоном, для генерации итоговых файлов. 



Переделанный пример предыдущего скрипта, шаблона и файла с данными (все файлы находятся в каталоге 2_example):

Шаблон templates/router_template.txt это обычный текстовый файл:
```
hostname {{name}}
!
interface Loopback10
 description MPLS loopback
 ip address 10.10.{{id}}.1 255.255.255.255
 !
interface GigabitEthernet0/0
 description WAN to {{name}} sw1 G0/1
!
interface GigabitEthernet0/0.1{{id}}1
 description MPLS to {{to_name}}
 encapsulation dot1Q 1{{id}}1
 ip address 10.{{id}}.1.2 255.255.255.252
 ip ospf network point-to-point
 ip ospf hello-interval 1
 ip ospf cost 10
!
interface GigabitEthernet0/1
 description LAN {{name}} to sw1 G0/2 !
interface GigabitEthernet0/1.{{IT}}
 description PW IT {{name}} - {{to_name}}
 encapsulation dot1Q {{IT}}
 xconnect 10.10.{{to_id}}.1 {{id}}11 encapsulation mpls
 backup peer 10.10.{{to_id}}.2 {{id}}21
  backup delay 1 1
!
interface GigabitEthernet0/1.{{BS}}
 description PW BS {{name}} - {{to_name}}
 encapsulation dot1Q {{BS}}
 xconnect 10.10.{{to_id}}.1 {{to_id}}{{id}}11 encapsulation mpls
  backup peer 10.10.{{to_id}}.2 {{to_id}}{{id}}21
  backup delay 1 1
!
router ospf 10
 router-id 10.10.{{id}}.1
 auto-cost reference-bandwidth 10000
 network 10.0.0.0 0.255.255.255 area 0
 !
```

Файл с данными routers_info.yml
```
- id: 11
  name: Liverpool
  to_name: LONDON
  IT: 791
  BS: 1550
  to_id: 1

- id: 12
  name: Bristol
  to_name: LONDON
  IT: 793
  BS: 1510
  to_id: 1

- id: 14
  name: Coventry
  to_name: Manchester
  IT: 892
  BS: 1650
  to_id: 2
```


Скрипт для генерации конфигураций router_config_generator_ver2.py
```python
# -*- coding: utf-8 -*-
from jinja2 import Environment, FileSystemLoader
import yaml

env = Environment(loader=FileSystemLoader('templates'))
template = env.get_template('router_template.txt')

routers = yaml.load(open('routers_info.yml'))

for router in routers:
    r1_conf = router['name']+'_r1.txt'
    with open(r1_conf,'w') as f:
        f.write(template.render(router))

```


Файл router_config_generator.py импортирует из модуля jinja2:
* __FileSystemLoader__ - загрузчик, который позволяет работать с файловой системой
 * тут указывается путь к каталогу, где находятся шаблоны
 * в данном случае, шаблон находится в каталоге templates
* __Environment__ - класс для описания параметров окружения:
 * в данном случае, указан только загрузчик
 * но в нем можно указывать методы обработки шаблона

Обратите внимание, что шаблон теперь находится в каталоге __templates__.

Если шаблоны находятся в текущем каталоге, надо добавить пару строк и изменить значение в загрузчике:
```python
import os

curr_dir = os.path.dirname(os.path.abspath(__file__))
env = Environment(loader=FileSystemLoader(curr_dir))
```

> Переменная ```__file__``` - это специальная переменная модуля, которая выставляется равной имени скрипта, который был запущен напрямую. И равна полному пути к модулю, когда он импортируется. [Подробнее о специальных переменных и методах](../16_additional_info/naming_conventions/underscore_names.md).

Метод __```get_template()```__ используется для того, чтобы получить шаблон. В скобках указывается имя файла.

Последняя часть осталась неизменной.

{% endraw %}
