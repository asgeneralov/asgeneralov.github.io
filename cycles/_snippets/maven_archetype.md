---
title: Стартовый проект Maven
layout: default
order: 1
---

# {{page.title}}

```
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart
```
Запускает интерактивный режим, в процессе которого сгенерирует maven-проект на базе архетипа quickstart. Это единственный архетип из мавенских, который не создаёт слишком много устаревшего кода и всякого другого лишнего. Можно использовать как заготовку для своего проекта. Спросят: имя проекта, группу, артефакт.

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
