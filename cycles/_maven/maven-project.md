---
title: Проект Maven
layout: default
order: 2
---

# {{ page.title}}
Чтобы создать проект Maven, нужно просто создать каталог с названием проекта и разместить в нём файл pom.xml, который будет описывать проект.
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
Файл начинается с заголовка, который является стандартным для формата XML и содержит версию схемы. Далее следует уникальный идентификатор артефакта, состоящий из группы, идентификатора и версии.
В теге _packaging_ указывается, как будет упакован будущий артефакт. Возможны следующие варианты:
* pom — артефакт представляет собой простой pom-файл;
* jar — стандартный формат для Java-библиотек;
* war — веб-архив, подходящий для развертывания на веб-сервере.

В теге _dependencies_ перечисляются сторонние библиотеки, необходимые для работы приложения, в уже знакомом формате.

## Модули. Наследование и агрегация
Структура проекта может быть разделена на отдельные модули, например, следующим образом:

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

Теперь, если запустить любую команду maven из корневой папки, то она автоматически выполнится для всех дочерних модулей.
Это свойство называется агрегацией. 

На данный момент модули все еще являются независимыми Maven проектами. Добавим в их pom-файлы информацию о родительском проекте.

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
Корневой pom, указанный в качестве родительского, не может быть представлен никаким другим артефактом, кроме как pom.
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
Все библиотеки, указанные в корневом pom, становятся библиотеками каждого модуля. Группа и версия артефакта для модуля берутся из корневого pom. 
 Это свойство называется наследованием.

Как видно из примера, свойства также наследуются в модулях.

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


### Перечень материалов
> [Introduction to the POM](https://maven.apache.org/guides/introduction/introduction-to-the-pom.html)  




 







 




