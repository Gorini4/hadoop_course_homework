1. Склонируйте репозиторий командой "git clone https://github.com/Gorini4/hadoop_course_homework.git"
2. Перейдите в директорию hadoop_course_homework/hw0
3. Запустите контейнеры командой "docker compose up"
4. Подождите (примерно две минуты), пока на экране не перестанут активно появляться новые строки логов.
5. Перейдите по адресу localhost:8088 - вы должны увидеть веб-интерфейс YARN.
6. После этого перейдите внутрь контейнера командой "docker exec -it namenode" и выполните последовательно две команды.
7. "curl -O https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-mapreduce-examples/3.2.1/hadoop-mapreduce-examples-3.2.1.jar" - для скачивания файла с приложением
8. "hadoop jar hadoop-mapreduce-examples-3.2.1.jar pi 2 20" - для запуска MapReduce-приложения
9. После запуска дождитесь его завершения и повторно проверьте интерфейс YARN. В нем должна появиться строка о выполнении приложения.
10. Скриншот интерфейса YARN с записью о запущенном приложении пришлите в чат с преподавателем.