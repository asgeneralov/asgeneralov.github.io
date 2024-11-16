---
title: Проект Maven
layout: default
order: 2
---

# {{ page.title}}
Для создания проекта Maven достаточно создать каталог с именем проекта и положить в него pom.xml &mdash; файл с описанием проекта. 
```
/my-app
  + pom.xml
```
pom.xml
```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  
  <!-- Уникальный идентификатор артефакта -->
  <groupId>com.mycompany.app</groupId>
  <artifactId>my-app</artifactId>
  <version>00.000.00</version>
  
  <!-- тип артефакта --> 
  <packaging>pom</packaging>

  <!-- список библиотек проекта -->
  <dependencies>
    ...
    <dependency>
      <groupId>third.party</groupId>
      <artifactId>lib</artifactId>
      <version>1.0.3</version>
    </dependency>
    ...
  </dependences>
  
</project>
```
Файл начинается со стандартной для xml формата шапки с указанием версии схемы. 
Далее уникалный идентификатор артефакта, состоящий из группы, идентификатора артефакта и его версии.
В теге _packaging_ указывается, как нужно упаковать будующий артефакт. Возможные варианты:
 - pom - артефакт является простым pom файлом
 - jar - стандартный формат java-библиотеки
 - war - веб-архив, формат пригодный для развертывания на веб сервере

В теге _dependencies_ в уже знакомом формате перечисляются сторонние библиотки используемые для работы приложения.

## Модули. Наследование и агрегация
Стуктура проекта может быть разделена на модули, например, таким образом:

```
/my-app
  /module-one
    + pom.xml
  /module-two
    + pom.xml
  pom.xml
```
Сама по себе структура еще ничего не дает. Все модули сейчас являются независимыми друг от друга.
Добавим в корневой pom информацию о модулях.

```
<project>
    ...
    <modules>
        <module>module-one</module>
        <module>module-two</module>
    </modules>
    ...    
</project>
```

Теперь, если выполнить любую команду maven, то она будет автоматически выполненна и для всех дочерних модулей.
Это свойство называется агрегацией. 

Пока дочерние модули ничего не знают про существование коренвого pom файла. Добавим в них информацию о корневом pom.

```
<!-- корневой pom -->
<project>
  <parent>
      <groupId>com.mycompany.app</groupId>
      <artifactId>my-app</artifactId>
      <version>00.000.00</version>
  </parent>
  <artifactId>module-one</artifactId>
</project>
<!-- pom для module-one -->
<project>
  <parent>
      <groupId>com.mycompany.app</groupId>
      <artifactId>my-app</artifactId>
      <version>00.000.00</version>
  </parent>
  <artifactId>module-one</artifactId>
</project>
```
Корневой pom, указанный как родительский, не может представлять никакой артефакт кроме как pom.
```
<project>
    ...
    <modules>
        <module>module-one</module>
        <module>module-two</module>
    </modules>
    
    <packaging>pom</packaging>
    ...    
</project>
```
Все библиотеки указанные в корневом pom становятся библиотеками каждого из модулей, а
группа и версия арефакта для модуля берется из корневого pom. Это свойство назывется наследованием.

В верхней части pom можно определить т.н. свойства _propery_, на которые можно ссылаться в других частях файла.

```
<project>
...
    <modules>
        ...
    </modules>
    <properties>
        <some.property.name>1.2.4</some.property.name>
        <other.property.name>8.9.3</other.property.name>
    </properties>
...
</project>

<project>
    ...   
    <artifactId>module-one</artifactId>
    ...
    <dependencies>
        <dependency>
          <groupId>some.vendor</groupId>
          <artifactId>util</artifactId>
          <!-- Возьмёт значение 8.9.3 из родительского pom -->
          <version>${some.property.name}</version>
        </dependency>
    </dependencies>
</project>

```

Как видно из примера свойства тоже наследуются в модулях.


### Список материалов
> [Introduction to the POM](https://maven.apache.org/guides/introduction/introduction-to-the-pom.html)  




 







 




