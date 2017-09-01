## Кортеж (Tuple)
Кортеж - это неизменяемый упорядоченный тип данных.

Кортеж в Python - это последовательность элементов, которые разделены между собой запятой и заключены в скобки.

Грубо говоря, кортеж - это список, который нельзя изменить. То есть, в кортеже есть только права на чтение. Это может быть защитой от случайных изменений.


Создать пустой кортеж:
```python
In [1]: tuple1 = tuple()

In [2]: print(tuple1)
()
```

Кортеж из одного элемента (обратите внимание на запятую):
```python
In [3]: tuple2 = ('password',)
```

Кортеж из списка:
```python
In [4]: list_keys = ['hostname', 'location', 'vendor', 'model', 'IOS', 'IP']

In [5]: tuple_keys = tuple(list_keys)

In [6]: tuple_keys
Out[6]: ('hostname', 'location', 'vendor', 'model', 'IOS', 'IP')
```

К объектам в кортеже можно обращаться, как и к объектам списка, по порядковому номеру:
```python
In [7]: tuple_keys[0]
Out[7]: 'hostname'
```

Но так как кортеж неизменяем, присвоить новое значение нельзя:
```python
In [8]: tuple_keys[1] = 'test'
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-9-1c7162cdefa3> in <module>()
----> 1 tuple_keys[1] = 'test'

TypeError: 'tuple' object does not support item assignment
```

