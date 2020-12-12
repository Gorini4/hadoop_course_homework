# Домашняя работа "Знакомство с HDFS"

## Для выполнения понадобятся

* Docker
* Docker Compose
* make

## Подготовка инфраструктуры

1. Запустите контейреры, зайдя в папку hw1 и выполнив команду `docker-compose up`
1. Добавьте в файл hosts (в *nix системах это файл `/etc/hosts`) следующие записи для возможности работать с Hadoop локально

``` text
127.0.0.1 namenode
127.0.0.1 datanode
```

## Подготовка данных

Выполните команду

``` text
docker exec namenode hdfs dfs -put /sample_data/stage /
```

В результате в корневой папке появится папка stage с тестовыми данными:

``` text
> docker exec namenode hdfs dfs -ls /stage
Found 3 items
drwxr-xr-x   - root supergroup          0 2020-12-12 00:35 /stage/date=2020-12-01
drwxr-xr-x   - root supergroup          0 2020-12-12 00:35 /stage/date=2020-12-02
drwxr-xr-x   - root supergroup          0 2020-12-12 00:35 /stage/date=2020-12-03
```

## Задача

Требуется написать Scala-приложение, которое будет очищать данные из папки `/stage` и складывать их в папку `/ods` в корне HDFS по следующим правилам:

1. Структура партиций (папок вида date=...) должна сохраниться.
1. Внутри папок должен остаться только один файл, содержащий все данные файлов из соответствующей партиции в папке `/stage`.

То есть, если у нас есть папка `/stage/date=2020-11-11` с файлами
``` text
part-0000.csv
part-0001.csv
```
то должна получиться папка `/ods/date=2020-11-11` с одним файлом
``` text
part-0000.csv
```
содержащим все данные из файлов папки `/stage/date=2020-11-11`.

**При выполнении нельзя использовать Spark, даже если вы уже с ним знакомы ;)**

## Подсказки

* Файлы конфигурации Hadoop можно найти внутри контейнера namenode в папке `/etc/hadoop`. Эти файлы нужно поместить в папку resources вашего проекта. И тогда при создании клиента они найдутся автоматически.
* Для взаимодействия с HDFS из Scala следует использовать [FileSystem API](https://hadoop.apache.org/docs/stable/api/org/apache/hadoop/fs/FileSystem.html)
* Для подключения этого API в ваш проект понадобится Hadoop Client:

``` scala
"org.apache.hadoop" % "hadoop-client" % "3.2.1"
```

* Для начала работы необходимо создать файловую систему

``` scala
import org.apache.hadoop.conf._
import org.apache.hadoop.fs._
import java.net.URI

val conf = new Configuration()
val fileSystem = FileSystem.get(new URI("hdfs://localhost:9000"), conf)

val path = new Path(filename)
fileSystem.open(path)
```

* В качестве примера можно использовать этот [репозиторий](https://github.com/ExNexu/hdfs-scala-example/blob/master/src/main/scala/HDFSFileService.scala)
* Спикок докер-контейнеров можно получить командой `docker ps`
* Попасть внутрь докер-контейнера можно командой `docker exec -it <container_name>`
* Любые возникающие вопросы можно обсудить в канале Slack вашей группы
