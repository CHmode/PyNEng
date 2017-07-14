### Выполнение команд SQL

Для выполнения команд SQL в модуле есть несколько методов:
* __```execute()```__ - метод для выполнения одного выражения SQL
* __```executemany()```__ - метод позволяет выполнить одно выражение SQL для последовательности параметров (или для итератора)
* __```executescript()```__ - метод позволяет выполнить несколько выражений SQL за один раз


#### Метод execute

Метод execute позволяет выполнить одну команду SQL.


Сначала надо создать соединение и курсор:
```python
In [1]: import sqlite3

In [2]: connection = sqlite3.connect('sw_inventory.db')

In [3]: cursor = connection.cursor()
```

Создание таблицы switch с помощью метода execute:
```python
In [4]: cursor.execute("create table switch (mac text primary key, hostname text, model text, location text)")
Out[4]: <sqlite3.Cursor at 0x1085be880>
```

Выражения SQL могут быть параметризированы - вместо данных можно подставлять специальные значения.
Засчет этого можно использовать одну и ту же команду SQL для передачи разных данных.


Например, таблицу switch нужно заполнить данными из списка data:
```python
In [5]: data = [
   ...: ('0000.AAAA.CCCC', 'sw1', 'Cisco 3750', 'London, Green Str'),
   ...: ('0000.BBBB.CCCC', 'sw2', 'Cisco 3780', 'London, Green Str'),
   ...: ('0000.AAAA.DDDD', 'sw3', 'Cisco 2960', 'London, Green Str'),
   ...: ('0011.AAAA.CCCC', 'sw4', 'Cisco 3750', 'London, Green Str')]
```

Для этого можно использовать запрос вида:
```python
In [6]: query = "INSERT into switch values (?, ?, ?, ?)"
```

Знаки вопроса в команде используются для подстановки данных, которые будут передаваться методу execute.

Теперь можно передать данные таким образом:
```
In [7]: for row in data:
   ...:     cursor.execute(query, row)
   ...:
```

Второй аргумент, который передается методу execute, должен быть кортежем.
Если нужно передать кортеж с одним элементом, используется запись ```(value, )```.

Чтобы изменения применились, нужно выполнить commit (обратите внимание, что метод commit вызывается у соединения):
```
In [8]: connection.commit()
```

Теперь, при запросе из командной строки sqlite3, можно увидеть эти строки в таблице switch:
```
$ sqlite3 sw_inventory.db

sqlite> select * from switch;
mac             hostname    model       location
--------------  ----------  ----------  -----------------
0000.AAAA.CCCC  sw1         Cisco 3750  London, Green Str
0000.BBBB.CCCC  sw2         Cisco 3780  London, Green Str
0000.AAAA.DDDD  sw3         Cisco 2960  London, Green Str
0011.AAAA.CCCC  sw4         Cisco 3750  London, Green Str
```

#### Метод executemany

Метод executemany позволяет выполнить одну команду SQL для последовательности параметров (или для итератора).

С помощью метода executemany, в таблицу switch можно добавить аналогичный список данных одной командой.

Например, в таблицу switch надо добавить данные из списка data2:
```python
In [9]: data2 = [
   ...: ('0000.1111.0001', 'sw5', 'Cisco 3750', 'London, Green Str'),
   ...: ('0000.1111.0002', 'sw6', 'Cisco 3750', 'London, Green Str'),
   ...: ('0000.1111.0003', 'sw7', 'Cisco 3750', 'London, Green Str'),
   ...: ('0000.1111.0004', 'sw8', 'Cisco 3750', 'London, Green Str')]
```

Для этого нужно использовать аналогичный запрос вида:
```python
In [10]: query = "INSERT into switch values (?, ?, ?, ?)"
```

Теперь можно передать данные методу executemany:
```python
In [11]: cursor.executemany(query, data2)
Out[11]: <sqlite3.Cursor at 0x10ee5e810>

In [12]: connection.commit()
```

После выполнения commit, данные доступны в таблице:
```
sqlite> select * from switch;
mac             hostname    model       location
--------------  ----------  ----------  -----------------
0000.AAAA.CCCC  sw1         Cisco 3750  London, Green Str
0000.BBBB.CCCC  sw2         Cisco 3780  London, Green Str
0000.AAAA.DDDD  sw3         Cisco 2960  London, Green Str
0011.AAAA.CCCC  sw4         Cisco 3750  London, Green Str
0000.1111.0001  sw5         Cisco 3750  London, Green Str
0000.1111.0002  sw6         Cisco 3750  London, Green Str
0000.1111.0003  sw7         Cisco 3750  London, Green Str
0000.1111.0004  sw8         Cisco 3750  London, Green Str
```

Метод executemany подставил соответствующие кортежи в команду SQL и все данные добавились в таблицу.


#### Метод executescript

Метод executescript позволяет выполнить несколько выражений SQL за один раз.


Особенно удобно использовать этот метод при создании таблиц:
```python
In [13]: connection = sqlite3.connect('new_db.db')

In [14]: cursor = connection.cursor()

In [15]: cursor.executescript("""
    ...:     create table switches(
    ...:         hostname     text primary key,
    ...:         location     text
    ...:     );
    ...:
    ...:     create table dhcp(
    ...:         mac          text primary key,
    ...:         ip           text,
    ...:         vlan         text,
    ...:         interface    text,
    ...:         switch       text not null references switches(hostname)
    ...:     );
    ...: """)
Out[15]: <sqlite3.Cursor at 0x10efd67a0>
```


