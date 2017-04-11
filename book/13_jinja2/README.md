#Шаблоны конфигураций с Jinja

[Jinja2](http://xgu.ru/wiki/Jinja2) это язык шаблонов, который используется в Python.
Jinja это не единственный язык шаблонов (шаблонизатор) для Python и, конечно же, не единственный язык шаблонов в целом.

Jinja2 используется для генерации документов на основе одного или нескольких шаблонов.

Примеры использования:
* шаблоны для генерации HTML-страниц
* шаблоны для генерации конфигурационных файлов в Unix/Linux
* шаблоны для генерации конфигурационных файлов сетевых устройств


Установить Jinja2 можно с помощью pip:
```python
pip install jinja2
```

> Далее термины Jinja и Jinja2 используются взаимозаменяемо.

Идея Jinja очень проста: разделение данных и шаблона.
Это позволяет использовать один и тот же шаблон, но подставлять в него разные данные.

В самом простом случае, шаблон это просто текстовый файл, в котором указаны места подстановки значений, с помощью переменных Jinja.

Пример шаблона Jinja:
```jinja
hostname {{name}}
!
interface Loopback255
 description Management loopback
 ip address 10.255.{{id}}.1 255.255.255.255
!
interface GigabitEthernet0/0
 description LAN to {{name}} sw1 {{int}}
 ip address {{ip}} 255.255.255.0
!
router ospf 10
 router-id 10.255.{{id}}.1
 auto-cost reference-bandwidth 10000
 network 10.0.0.0 0.255.255.255 area 0
```

Комментарии к шаблону:
* В Jinja переменные записываются в двойных фигурных скобках.
* При выполнении скрипта, эти переменные заменяются нужными значениями.

Этот шаблон может использоваться для генерации конфигурации разных устройств, с помощью подстановки других наборов переменных.

Пример скрипта с генерацией файла на основе шаблона Jinja (файл basic_generator.py):
```python
from jinja2 import Template

template = Template(u"""
hostname {{name}}
!
interface Loopback255
 description Management loopback
 ip address 10.255.{{id}}.1 255.255.255.255
!
interface GigabitEthernet0/0
 description LAN to {{name}} sw1 {{int}}
 ip address {{ip}} 255.255.255.0
!
router ospf 10
 router-id 10.255.{{id}}.1
 auto-cost reference-bandwidth 10000
 network 10.0.0.0 0.255.255.255 area 0
""")

liverpool = {'id':'11', 'name':'Liverpool', 'int':'Gi1/0/17', 'ip':'10.1.1.10'}

print template.render( liverpool )
```

Комментарии к файлу basic_generator.py:
* в первой строке из Jinja2 импортируется класс Template
* создается объект template, которому передается шаблон
 * в шаблоне используются переменные в синтаксисе Jinja
* в словаре liverpool ключи должны быть такими же, как имена переменных в шаблоне
 * значения, которые соответствуют ключам, это те данные, которые будут подставлены на место переменных
* последняя строка рендерит шаблон используя словарь liverpool, то есть, подставляет значения в переменные.

Если запустить скрипт basic_generator.py, то вывод будет таким:
```
$ python basic_generator.py

hostname Liverpool
!
interface Loopback255
 description Management loopback
 ip address 10.255.11.1 255.255.255.255
!
interface GigabitEthernet0/0
 description LAN to Liverpool sw1 Gi1/0/17
 ip address 10.1.1.10 255.255.255.0
!
router ospf 10
 router-id 10.255.11.1
 auto-cost reference-bandwidth 10000
 network 10.0.0.0 0.255.255.255 area 0
```

 
