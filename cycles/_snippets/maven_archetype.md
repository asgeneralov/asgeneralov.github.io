---
title: Стартовый проект Maven
layout: default
order: 1
---

# {{page.title}}

```
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart
```
Запускает интерактивный режим в процессе которого сгенерит maven-проект на базе архетипа quickstart. Это единственный архетип из мавенских, который не создаёт слишком много устарешего кода и всякого другого лишнего. Можно использовать как заготовку для своего проекта.
Спросят: имя проекта, группу, артифакт.

## Структура проекта

```
project
|-- pom.xml
`-- src
    |-- main
    |   `-- java
    |       `-- $package
    |           `-- App.java
    `-- test
        `-- java
            `-- $package
                `-- AppTest.java
```
