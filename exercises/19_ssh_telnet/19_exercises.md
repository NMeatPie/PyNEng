# Задания

{% include "../exercises_intro.md" %}

### Задание 19.1

Создать функцию send_show_command.

Функция подключается по SSH (с помощью netmiko) к устройствам из списка, и выполняет  команду
на основании переданных аргументов.

Параметры функции:
* devices_list - список словарей с параметрами подключения к устройствам, которым надо передать команды
* command - команда, которую надо выполнить

Функция возвращает словарь с результатами выполнения команды:
* ключ - IP устройства
* значение - результат выполнения команды

Проверить работу функции на примере:
* устройств из файла devices.yaml (для этого надо считать информацию из файла)
* и команды command

```python
command = "sh ip int br"
```

### Задание 19.1a

Переделать функцию send_show_command из задания 19.1 таким образом,
чтобы обрабатывалось исключение, которое генерируется
при ошибке аутентификации на устройстве.

При возникновении ошибки, должно выводиться сообщение исключения.

Для проверки измените пароль на устройстве или в файле devices.yaml.


### Задание 19.1b

Дополнить функцию send_show_command из задания 19.1a таким образом,
чтобы обрабатывалось не только исключение, которое генерируется
при ошибке аутентификации на устройстве, но и исключение,
которое генерируется, когда IP-адрес устройства недоступен.

При возникновении ошибки, должно выводиться сообщение исключения.

Для проверки измените IP-адрес на устройстве или в файле devices.yaml.


### Задание 19.2

Создать функцию send_config_commands

Функция подключается по SSH (с помощью netmiko) к устройствам из списка, и выполняет перечень команд в конфигурационном режиме на основании переданных аргументов.

Параметры функции:
* devices_list - список словарей с параметрами подключения к устройствам, которым надо передать команды
* config_commands - список команд, которые надо выполнить

Функция возвращает словарь с результатами выполнения команды:
* ключ - IP устройства
* значение - вывод с выполнением команд

Проверить работу функции на примере:
* устройств из файла devices.yaml (для этого надо считать информацию из файла)
* и списка команд commands

```python

commands = [ 'logging 10.255.255.1',
             'logging buffered 20010',
             'no logging console' ]
```

### Задание 19.2a

Дополнить функцию send_config_commands из задания 19.2

Добавить аргумент output, который контролирует будет ли результат
выполнения команд выводиться на стандартный поток вывода.

По умолчанию, результат должен выводиться.


### Задание 19.2b

Переделать функцию send_config_commands из задания 19.2a или 19.2

Добавить проверку на ошибки:
* При выполнении команд, скрипт должен проверять результат на такие ошибки:
 * Invalid input detected, Incomplete command, Ambiguous command

Если при выполнении какой-то из команд возникла ошибка,
функция должна выводить сообщение на стандартный поток вывода с информацией
о том, какая ошибка возникла, при выполнении какой команды и на каком устройстве.

При этом, параметр output также должен работать, но теперь он отвечает за вывод
только тех команд, которые выполнились корректно.

Функция send_config_commands теперь должна возвращать кортеж из двух словарей:
* первый словарь с выводом команд, которые выполнились без ошибки
* второй словарь с выводом команд, которые выполнились с ошибками


Оба словаря в формате
* ключ - IP устройства
* значение -  вложенный словарь:
   * ключ - команда
   * значение - вывод с выполнением команд

Проверить функцию на команде с ошибкой.


### Задание 19.2c

Переделать функцию send_config_commands из задания 19.2b

Если при выполнении команды на одном из устройств возникла ошибка,
спросить пользователя надо ли выполнять эту команду на других устройствах.

Варианты ответа [y/n]:
* y - выполнять команду на оставшихся устройствах
* n - не выполнять команду на оставшихся устройствах

Функция send_config_commands по-прежнему должна возвращать кортеж из двух словарей:
* первый словарь с выводом команд, которые выполнились без ошибки
* второй словарь с выводом команд, которые выполнились с ошибками


Оба словаря в формате
* ключ - IP устройства
* значение -  вложенный словарь:
   * ключ - команда
   * значение - вывод с выполнением команд

Проверить функцию на команде с ошибкой.


### Задание 19.3

Создать функцию send_commands (для подключения по SSH используется netmiko).

Параметры функции:
* devices_list - список словарей с параметрами подключения к устройствам, которым надо передать команды
* show - одна команда show (строка)
* filename - имя файла, в котором находятся команды, которые надо выполнить (строка)
* config - список с командами, которые надо выполнить в конфигурационном режиме

В зависимости от того, какой аргумент был передан, функция вызывает разные функции внутри.
При вызове функции, всегда будет передаваться только один из аргументов show, config, filename.

Далее комбинация из аргумента и соответствующей функции:
* show - функция send_show_command из задания 19.1
* config - функция send_config_commands из задания 19.2, 12.2a или 12.2b
* filename - функция send_commands_from_file (ее также надо написать по аналогии с предыдущими)

Функция возвращает словарь с результатами выполнения команды:
* ключ - IP устройства
* значение - вывод с выполнением команд

Проверить работу функции на примере:
* устройств из файла devices.yaml (для этого надо считать информацию из файла)
* и различных комбинация аргумента с командами:
    * списка команд commands
    * команды command
    * файла config.txt

```python

commands = ['logging 10.255.255.1',
            'logging buffered 20010',
            'no logging console' ]
command = "sh ip int br"

```


### Задание 19.3a

Изменить функцию send_commands таким образом, чтобы в списке словарей device_list
не надо было указывать имя пользователя, пароль, и пароль на enable.

Функция должна запрашивать имя пользователя, пароль и пароль на enable при старте.
Пароль не должен отображаться при наборе.

В файле devices2.yaml эти параметры уже удалены.



### Задание 19.3b

Дополнить функцию send_commands таким образом, чтобы перед подключением к устройствам по SSH,
выполнялась проверка доступности устройства pingом (можно вызвать команду ping в ОС).

> Как выполнять команды ОС, описано в разделе [subprocess](../../book/08_modules/useful_modules/subprocess.md). Там же есть пример функции с отправкой ping.

Если устройство доступно, можно выполнять подключение.
Если не доступно, вывести сообщение о том, что устройство с определенным IP-адресом недоступно
и не выполнять подключение.

Для удобства можно сделать отдельную функцию для проверки доступности 
и затем использовать ее в функции send_commands.


### Задание 19.3c

Переделать задание 12.3b таким образом, чтобы проверка доступности устройств
выполнялась не последовательно, а параллельно.

Для этого, необходимо взять за основу функцию check_ip_addresses из задания 8.3.
Функцию надо переделать таким образом, чтобы проверка IP-адресов выполнялась
параллельно в разных потоках.

Функция check_ip_addresses ожидает как аргумент список IP-адресов.
И возвращает два списка:
* список доступных IP-адресов
* список недоступных IP-адресов

Если какие-то IP-адреса недоступны, функция send_commands должна
вывести сообщение с информацией о том, какие адреса недоступны.


### Задание 19.3d

Переделать функцию check_ip_addresses из задания 19.3c таким образом,
чтобы она позволяла контролировать количество параллельных проверок IP.

Для этого, необходимо добавить новый параметр limit,
со значением по умолчанию - 2.

Функция check_ip_addresses должна проверять адреса из списка
таким образом, чтобы в любой момент времени максимальное количество
параллельных проверок было равным limit.


### Задание 19.4

В задании используется пример из раздела про [модуль threading](../../book/20_concurrent_connections/5a_threading.md).

Переделать пример таким образом, чтобы:
* вместо функции connect_ssh, использовалась функция send_commands из задания 19.3
 * переделать функцию send_commands, чтобы использовалась очередь и функция conn_threads по-прежнему возвращала словарь с результатами.
 * Проверить работу со списком команд, с командами из файла, с командой show 

Подсказка: threading.Thread может передавать функции не только позиционные аргументы, но и ключевые:
```python
def conn_threads(function, arg1, arg2, **kwargs_dict):

    for some in something:
        th = threading.Thread(target=function,
                              args=(arg1, arg2),
                              kwargs=kwargs_dict)
```

Пример из раздела:
```python
import threading
from queue import Queue
from pprint import pprint
from netmiko import ConnectHandler


COMMAND = sys.argv[1]
devices = yaml.load(open('devices.yaml'))


def connect_ssh(device_dict, command, queue):
    with ConnectHandler(**device_dict) as ssh:
        ssh.enable()
        result = ssh.send_command(command)
        print("Connection to device {}".format(device_dict['ip']))

        #Добавляем словарь в очередь
        queue.put({device_dict['ip']: result})


def conn_threads(function, devices, command):
    threads = []
    q = Queue()

    for device in devices:
        # Передаем очередь как аргумент, функции
        th = threading.Thread(target=function, args=(device, command, q))
        th.start()
        threads.append(th)

    for th in threads:
        th.join()

    results = []
    # Берем результаты из очереди и добавляем их в список results
    for t in threads:
        results.append(q.get())

    return results

pprint(conn_threads(connect_ssh, devices['routers'], COMMAND))

```


### Задание 19.5

Использовать функции полученные в результате выполнения задания 19.4.

Переделать функцию conn_threads таким образом, чтобы с помощью аргумента limit,
можно было указывать сколько подключений будут выполняться параллельно.
По умолчанию, значение аргумента должно быть 2.

Изменить функцию соответственно, так, чтобы параллельных подключений выполнялось столько,
сколько указано в аргументе limit.

Теперь, если в сумме надо подключиться к 5 устройствам, а параметр limit = 2,
функция выполнит подключения сначала к 1 и 2 устройству параллельно,
затем аналогично к 3 и 4, и затем к 5.


### Задание 19.6

В задании используется пример из раздела про [модуль multiprocessing](book/chapter12/5b_multiprocessing.md).

Переделать пример таким образом, чтобы:
* вместо функции connect_ssh, использовалась функция send_commands из задания 19.3
 * переделать функцию send_commands, чтобы использовалась очередь и функция conn_processes по-прежнему возвращала словарь с результатами.
 * Проверить работу со списком команд, с командами из файла, с командой show

Подсказка: multiprocessing.Process может передавать функции не только позиционные аргументы, но и ключевые:
```python
def conn_processes(function, arg1, arg2, **kwargs_dict):

    for some in something:
        p = multiprocessing.Process(target=function,
                                    args=(arg1, arg2),
                                    kwargs=kwargs_dict)
```

Пример из раздела:
```python
import multiprocessing
import sys
import yaml
from pprint import pprint

from netmiko import ConnectHandler


COMMAND = sys.argv[1]
devices = yaml.load(open('devices.yaml'))


def connect_ssh(device_dict, command, queue):
    with ConnectHandler(**device_dict) as ssh:
        ssh.enable()
        result = ssh.send_command(command)

        print("Connection to device {}".format(device_dict['ip']))
        queue.put({device_dict['ip']: result})


def conn_processes(function, devices, command):
    processes = []
    queue = multiprocessing.Queue()

    for device in devices:
        p = multiprocessing.Process(target=function,
                                    args=(device, command, queue))
        p.start()
        processes.append(p)

    for p in processes:
        p.join()

    results = []
    for p in processes:
        results.append(queue.get())

    return results

pprint((conn_processes(connect_ssh, devices['routers'], COMMAND)))

```

### Задание 19.7

Использовать функции полученные в результате выполнения задания 19.6.

Переделать функцию conn_processes таким образом, чтобы с помощью аргумента limit,
можно было указывать сколько подключений будут выполняться параллельно.
По умолчанию, значение аргумента должно быть 2.

Изменить функцию соответственно, так, чтобы параллельных подключений выполнялось столько,
сколько указано в аргументе limit.

Теперь, если в сумме надо подключиться к 5 устройствам, а параметр limit = 2,
функция выполнит подключения сначала к 1 и 2 устройству параллельно,
затем аналогично к 3 и 4, и затем к 5.
