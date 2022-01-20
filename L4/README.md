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
<br/>
Передав дополнительно флаг ```-s```, мы указали, что создаваемый узел нумерованный. Этот способ позволяет создавать узлы с уникальными именами, по которым можно узнать порядок поступления запросов на сервер. <br/>
Пример. Принадлежность клиентов к группе. <br/>
Несмотря на то, что ZooKeeper используется, как правило, из программного кода, мы можем эмулировать простой сценарий мониторинга принадлежности клиентов к группе в CLI. <br/>
В данном примере в корне zookeeper файловой системы будет создан узел под именем mygroup. Затем несколько сессий CLI будут эмулировать клиентов, добавляющих себя в эту группу. Клиент будет добавлять эфимерный узел в mygroup иерархию. При закрытии сессии узел автоматически будет удаляться.
Этот сценарий может применяться для реализации сервиса разрешения имён (DNS) узлов кластера. Каждый узел регистрирует себя под своим именем и сохраняет свой url или ip адрес. Узлы, которые временно недоступны или аварийно завершили работу, в списке отсутствуют. Таким образом директория хранит актуальный список работающих узлов с их адресами. <br/>
Внутри CLI сессии, создадим узел ```mygroup```. 
Откроем две новых CLI консоли и в каждой создайте по дочернему узлу в ```mygroup``` и проверим, что ```grue``` и ```bleen``` являются членами группы ```mygroup```: <br/>
<br/>
<img src="/L4/images/grue_bleen.png" width="800"> <br/>
<br/>
Представим теперь, что одному из клиентов нужна информация о другом клиенте (к качестве клиентов могут выступать узлы кластера). Этот сценарий эмулируется получением информации командой get. Выберем консоль ```grue``` и обратимся к информации узла ```bleen```. <br/>
Информацией, которая хранится в узле клиента может быть ```url``` адрес клиента, либо любая другая информация требуемая для работы распределённого приложения. <br/>
Теперь эмулируем аварийное отключение любого клиента. Нажмем сочетание клавиш ```Ctrl + D``` в одной из консолей, создавшей эфимерный узел. <br/>
Проверим, что соответствующий узел пропал из ```mygroup```. Изменение списка дочерних узлов может произойти не сразу — от 2 до 20 ```tickTime```, значение которого можно посмотреть в ```zoo.cfg```.
<br/>
<img src="/L4/images/lost_grue.png" width="500"> <br/>
<br/>
Таким образом клиенты могут получать информацию о появлении и отключении других клиентов.
В заключении удалим узел ```/mygroup```.
<br/>
<img src="/L4/images/delete.png" width="500"> <br/>
<br/>
### Пример управления конфигурацией распределённого приложения
Хранение конфигурационной информации в ZooKeeper одно из наиболее популярных приложений. Мы будем эмулировать данную концепцию также с помощью CLI. <br/>
Использование ZooKeeper для хранения конфигурационной информации имеет два преимущества: 
+ Новые клиенты могут узнавать конфигурацию кластера и определять свою роль самостоятельно,
+ Возможность подписки на обновление конфигурационных параметров. Это позволяет динамически реагировать на изменения в конфигурации во время выполнения, что необходимо в режиме работы 24/7. <br/>


Создадим в корне узел ```"myconfig"```, в задачу которого будет входить хранение конфигурации. В нашем случае узел будет хранить строку ```'sheep_count=1'```.
Во всех случаях, когда конфигурационная информация нашего гипотетического распределённого приложения будет изменяться, мы будем обновлять ```znode``` строкой с новым значением. Другим клиентам распределённого приложения достаточно проверять хранимые в этом узле данные. <br/>
Откроем новую консоль и подключитесь к ZooKeeper. Данная консоль будет играть роль физического сервера, который ожидает получить оповещение в случае изменения конфигурационной информации, записанной в ```/myconfig``` znode. <br/>
Следующая команда устанавливает watch-триггер на узел: <br/>
```cmd 
get /myconfig -w true
``` 
</br>

Вернемся к первому терминалу и изменим значение ```myconfig```:
```cmd
set /myconfig 'sheep_count=2'
```
<br/>
Во втором терминале должно появиться оповещение об изменении данных! <br/>
<img src="/L4/images/myconfig_test.png" width="500"> <br/>

Триггер сбрасывается после одного срабатывания, а значит его придётся 'взводить' каждый раз заново. Как правило, в приложении, в логике обработчика события присутствует такая процедура. <br/>

Удалим узел ```/myconfig```и проверим, что эта команда выполнилась.

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
Client 0 request commit
Client 1 request abort
Client 4 request abort
Client 3 request abort
Client 2 request abort
Check clients
Client 0 do abort
Client 1 do abort
Client 2 do abort
Client 3 do abort
Client 4 do abort
Waiting others clients: []

Process finished with exit code 0
```
