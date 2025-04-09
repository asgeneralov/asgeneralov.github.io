---
layout: default 
title: "Динамическая загрузка классов"
order: 1
---
# {{page.title}}
Одна из ключевых особенностей Java, повлиявшая на популярность языка, возможность динамической загрузки классов.
Классы могу быть загружены из файловой системы, из архива, из библиотеки, по сети &mdash; откуда угодно.

Загрузка классов и ресурсов (картинки, конфигурационные файоы и проч.) осуществялется через объект ClassLoader.

Получить текущий загрузчик, которым был загружен класс исполняемый в данный момент
```
Thread.currenThread().getContextClassloader();
```
Так же его можно переопределить
```
Thread.currenThread().setContextClassLoader(myClassloader);
```
Загрузчики в Java образуют иерархию, у каждого из них есть родительский загрузчик (кроме BootstrapClassLoader'а). 
Если вместо загрузчика передать ***null***, то родитеским будет SystemClassLoader т.к. он запускает приложение  
```
ClassLoader.getSystemClassLoader()
ClassLoader.getPlatformClassLoader()
```

Как правило ккласс


```
ClassLoader {
    ClassLoader(String name, ClassLoader parent)
    Class<?> loadClass(String name)
    URL getResource(String name)
    Stream<URL> resources(String name)
    ClassLoader getParent()
}
```