### Система управления пакетами pip

Для установки пакетов Python, будет использоваться pip. Это система управления пакетами, которая используется для установки пакетов из Python Package Index (PyPi). Скорее всего, если у Вас уже установлен Python, то установлен и pip.

Проверка версии pip:

```
$ pip --version
pip 9.0.1 from /home/vagrant/venv/py3_convert/lib/python3.6/site-packages (python 3.6)
```

Если команда выдала ошибку, значит, pip не установлен. Установка pip описана в [документации](https://pip.pypa.io/en/stable/installing/)

#### Установка модулей

Для установки модулей используется команда pip install:

```
$ pip install tabulate

Collecting tabulate
Installing collected packages: tabulate
Successfully installed tabulate-0.8.1
```

Удаление пакета выполняется таким образом:

```
$ pip uninstall tabulate
```

Кроме того, иногда необходимо обновить пакет:

```
$ pip install --upgrade netmiko
```

#### pip или pip3

В зависимости от того, как установлен и настроен Python в системе, может потребоваться использовать pip3, вместо pip. Чтобы проверить какой вариант используется, надо выполнить команду pip --version.

Вариант, когда pip соответствует Python 2.7:

```
$ pip --version
pip 9.0.1 from /usr/local/lib/python2.7/dist-packages (python 2.7)
```

Вариант, когда pip3 соответствует Python 3.4:

```
$ pip3 --version
pip 1.5.6 from /usr/lib/python3/dist-packages (python 3.4)
```

Если в системе используется pip3, то каждый раз, когда в книге устанавливается модуль Python, нужно будет заменить pip на pip3.

Также можно использовать альтернативный вариант вызова pip:

```
$ python3.6 -m pip install tabulate
```

Таким образом, всегда понятно для какой именно версии Python устанавливается пакет.
