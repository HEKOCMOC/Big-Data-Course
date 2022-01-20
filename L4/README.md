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
Для запуска же: ```zkServer start```

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

## Разработка, запуск и проверка работы приложения
1. Решите проблему обедающих философов <br/>
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
2. Реализуйте двуфазный коммит протокол для high-available регистр <br/>
Результат выполнения:
