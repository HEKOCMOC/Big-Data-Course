# Лабораторная работа №4 - ZooKeeper 👨‍🌾
## Задание
+ запустить ZooKeeper,
+ изучить директорию с установкой ZooKeeper,
+ запустить интерактивную сессию ZooKeeper CLI и освоить её команды,
+ научиться проводить мониторинг ZooKeeper,
+ разработать приложение с барьерной синхронизацией, основанной на ZooKeeper,
+ запустить и проверить работу приложения.

## Установка и запуск
Для установки использовалась следующая команда: ```brew install zookeeper``` <br/>
<img src="/L4/images/install.png" width="500"> <br/>
Для запуска же: ```zkServer start``` <br/>
<img src="/L4/images/start.png" width="500">
## Изучение директории
Перейдем в директорию установки ZooKeeper и изучим содержимое директории. <br/>
В директории находятся следующие папки:
+ bin с исполняемыми файлами для запуска, остановки и взаимодействия с ZooKeeper,
+ conf с конфигурационными файлами,
+ contrib с инструментами для интеграции ZooKeeper в другие системы: rest, fuse, perl и python библиотеки,
+ dist-maven артефакты Maven,
+ docs в которой хранится документация,
+ recipes различные рецепты, помогающие решать задачи с использованием ZooKeeper (выбор лидера, блокировки, очереди),
+ src с исходным кодом и тестовыми скриптами.

## Запуск интерактивной сессии ZooKeeper CLI, освоение команд
Для запуска интерактивной сессии ZooKeeper CLI используем скрипт zkCli.
Следующая команда устанавливает подключение к ZooKeeper CLI сессии: <br/>
<img src="/L4/images/zkCli.png" width="500"> <br/>
Подключение установлено. Для вывода всех возможных команд наберем ```help```:
<img src="/L4/images/help.png" width="500"> <br/>
Далее последует изучение возможностей CLI интерфейса. Научимся добавлять и удалять разные типы узлов znode, считывать и записывать данные в znode из CLI, разбираться в управлении конфигурациями на базовых примерах.
Находясь в консоли CLI введем команду ```ls /```.
В результе получим список узлов в корне иерархической структуры данных ZooKeeper. В данном случае выводится один узел. Аналогично можно изучать некорневые узлы. Выведем список дочерних узлов ```/zookeeper```: <br/>
<br/>
<img src="/L4/images/ls_zookeeper.png" width="500"> <br/>
Теперь в корне создим свой узел ```/mynode``` с данными ```"first_version"``` следующей командой ```create /mynode 'first_version'```.
Проверяем, что в корне появился новый узел: <br/>
<br/>
<img src="/L4/images/create_my_node.png" width="500"> <br/>
Изменим данные узла на ```"second_version"```: <br/>
<br/>
<img src="/L4/images/second_version.png" width="500"> <br/>
Создадим два нумерованных (sequential) узла в качестве дочерних ```mynode```: <br/>
<br/>
<img src="/L4/images/childs.png" width="500"> <br/>

## Разработка, запуск и проверка работы приложения
1. Решить проблему обедающих философов <br/>
Результат выполнения :
```scala
Philosopher 2 is going to eat
Philosopher 1 is going to eat
Philosopher 0 is going to eat
Philosopher 0 picked up the left fork
Philosopher 0 picked up the right fork
Philosopher 0 put the right fork
Philosopher 1 picked up the left fork
Philosopher 1 picked up the right fork
Philosopher 0 put the loft fork and finished eating
Philosopher 0 is thinking
Philosopher 2 picked up the left fork
Philosopher 1 put the right fork
Philosopher 1 put the loft fork and finished eating
Philosopher 2 picked up the right fork
Philosopher 1 is thinking
Philosopher 0 is going to eat
Philosopher 1 is going to eat
Philosopher 0 picked up the left fork
Philosopher 2 put the right fork
Philosopher 0 picked up the right fork
Philosopher 2 put the loft fork and finished eating
Philosopher 2 is thinking
Philosopher 1 picked up the left fork
Philosopher 0 put the right fork
Philosopher 1 picked up the right fork
Philosopher 0 put the loft fork and finished eating
Philosopher 0 is thinking
Philosopher 1 put the right fork
Philosopher 1 put the loft fork and finished eating
Philosopher 1 is thinking
Philosopher 2 is going to eat
Philosopher 2 picked up the left fork
Philosopher 2 picked up the right fork
Philosopher 2 put the right fork
Philosopher 2 put the loft fork and finished eating
Philosopher 2 is thinking

Process finished with exit code 0
```
2. Реализовать двуфазный коммит протокол для high-available регистр <br/>
Результат выполнения:
```python
Waiting others clients: []
Client 1 request abort
Client 3 request commit
Client 0 request abort
Client 2 request abort
Client 4 request commit
Check clients
Client 0 do abort
Client 1 do abort
Client 2 do abort
Client 3 do abort
Client 4 do abort
Waiting others clients: []
```
