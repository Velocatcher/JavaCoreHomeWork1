# Домашнее задание: #

– Создать проект из трёх классов (основной с точкой входа и два класса в дру-
гом пакете), которые вместе должны составлять одну программу, позволяющую
производить четыре основных математических действия и осуществлять форматированный
вывод результатов пользователю.
****
– Скомпилировать проект, а также создать для этого проекта стандартную веб-
страницу с документацией ко всем пакетам.

javac -d out -sourcepath ./src ./src/sample/Main.java

java -classpath ./out sample.Main

javadoc -d docs -sourcepath ./src -cp ./out -subpackages regular sample

****
– Создать Makefile с задачами сборки, очистки и создания документации на весь
проект.

SRC_DIR := src\
OUT_DIR := out\
DOC_DIR := doc

JC := javac\
JDOC := javadoc\
JSRC := -sourcepath ./$(SRC_DIR)\
JCLASS := -cp ./$(OUT_DIR)\
JCDEST := -d $(OUT_DIR)\
JDOCDEST := -d $(DOC_DIR)\
MAIN_SOURCE := src/sample/Main\
MAIN_CLASS := src.sample.Main

all:\
    ${JC} ${JSRC} ${JCDEST} ${SRC_DIR}/${MAIN_SOURCE}.java

clean:\
    rm -R ${OUT_DIR}

run:\
    cd out && java ${MAIN_CLASS}

docs:\
    ${JDOC}) ${JDOCDEST} ${JSRC} ${JCLASS} -subpackages ru
    
– *Создать два Docker-образа. Один должен компилировать Java-проект обратно в
папку на компьютере пользователя, а второй забирать скомпилированные клас-
сы и исполнять их.

Вариант решения
Для упрощения был использован docker compose, вместо чистого Docker. Фай-
лы, компилирующие и исполняющие программу представлены в листингах ни-
же. Оба эти файла запускаются из корня папки проекта командами

docker compose -f docker-compose-class.yml up\
docker compose -f docker-compose-exec.yml up\
Листинг 7: docker-compose-class.yml\
services:\
 app:\
 image: bellsoft/liberica-openjdk-alpine:11.0.16.1-1\
 command: javac -sourcepath /app/src -d /app/out /app/src/sample/Main.java\
 volumes:\
 -./bin:/app/out\
 -./src:/app/src

Листинг 8: docker-compose-exec.yml\
 services:\
   app:\
     image: bellsoft/liberica-openjdk-alpine:11.0.16.1-1\
   command: java -classpath /app/out src.sample.Main\
   volumes:\
     - ./bin:/app/out

