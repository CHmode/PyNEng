#### Словарь из двух списков (advanced)

> Слово 'advanced' в заголовке, означает, что то, что рассматривается в этом разделе ещё не было в темах курса. Но, в дальнейшем, такой прием может пригодится.

В разделе [варианты создания словаря](./03_data_structures/6b_create_dict.md) рассматривался такой вариант создания словаря:
```python
In [4]: r1 = dict([('model','4451'), ('IOS','15.4')])

In [5]: r1
Out[5]: {'IOS': '15.4', 'model': '4451'}
```


Как правило, такой способ создания используется не в том случае, когда словарь создается вручную в текстовом файле, а когда словарь создается из каких-то других промежуточных объектов.

Воспользуемся таким способом создания словаря, чтобы объединить два списка одинаковой длины в словарь.

Для этого воспользуемся функцией __```zip()```__, которая возвращает список кортежей:
```python
In [1]: a = [1,2,3]

In [2]: b = [100,200,300]

In [3]: zip(a,b)
Out[3]: [(1, 100), (2, 200), (3, 300)]
```

Теперь на более полезном примере:
```python
In [4]: d_keys = ['hostname', 'location', 'vendor', 'model', 'IOS', 'IP']
In [5]: d_values = ['london_r1', '21 New Globe Walk', 'Cisco', '4451', '15.4', '10.255.0.1']

In [6]: zip(d_keys,d_values)
Out[6]: 
[('hostname', 'london_r1'),
 ('location', '21 New Globe Walk'),
 ('vendor', 'Cisco'),
 ('model', '4451'),
 ('IOS', '15.4'),
 ('IP', '10.255.0.1')]

In [7]: dict(zip(d_keys,d_values))
Out[7]: 
{'IOS': '15.4',
 'IP': '10.255.0.1',
 'hostname': 'london_r1',
 'location': '21 New Globe Walk',
 'model': '4451',
 'vendor': 'Cisco'}
In [8]: r1 = dict(zip(d_keys,d_values))

In [9]: r1
Out[9]: 
{'IOS': '15.4',
 'IP': '10.255.0.1',
 'hostname': 'london_r1',
 'location': '21 New Globe Walk',
 'model': '4451',
 'vendor': 'Cisco'}
```


В примере ниже есть отдельный список, в котором хранятся ключи, и словарь, в котором хранится в виде списка (чтобы сохранить порядок) информация о каждом устройстве.

Соберем их в словарь с ключами из списка и информацией из словаря data:
```python
In [10]: d_keys = ['hostname', 'location', 'vendor', 'model', 'IOS', 'IP']

In [11]: data = {
   ....: 'r1': ['london_r1', '21 New Globe Walk', 'Cisco', '4451', '15.4', '10.255.0.1'],
   ....: 'r2': ['london_r2', '21 New Globe Walk', 'Cisco', '4451', '15.4', '10.255.0.2'],
   ....: 'sw1': ['london_sw1', '21 New Globe Walk', 'Cisco', '3850', '3.6.XE', '10.255.0.101']
   ....: }

In [12]: london_co = {}

In [13]: for k in data.keys():
   ....:     london_co[k] = dict(zip(d_keys,data[k]))
   ....:     

In [14]: london_co
Out[14]: 
{'r1': {'IOS': '15.4',
  'IP': '10.255.0.1',
  'hostname': 'london_r1',
  'location': '21 New Globe Walk',
  'model': '4451',
  'vendor': 'Cisco'},
 'r2': {'IOS': '15.4',
  'IP': '10.255.0.2',
  'hostname': 'london_r2',
  'location': '21 New Globe Walk',
  'model': '4451',
  'vendor': 'Cisco'},
 'sw1': {'IOS': '3.6.XE',
  'IP': '10.255.0.101',
  'hostname': 'london_sw1',
  'location': '21 New Globe Walk',
  'model': '3850',
  'vendor': 'Cisco'}}
```

