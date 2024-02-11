---
layout: default 
title: "Свойства в Java"
---
# {{page.title}}

Свойствами (англ. properties) называют список параметров типа ключ-значение, доступных для чтения в работающем приложении. Основное назначение _свойств_ &mdash; сообщать программе характеристики среды в которой она запущена или же изменить настройки приложения. Например, с помощью _свойств_ можно включить режим отладки, передать адрес прокси-сервера или базы данных.

## Свойства окружения (анг. environment properties)
Свойствами окружения назвают параметры операционной системы, которые устанавливаются администратором или пользователем ПО. Администратор может устанавливать глобальные свойства окружения, которые будут видны всем пользователям. Каждый пользователь ОС может устанавливать свои переменные окружения в дополнение к глобальным. Кроме того, при работе в командной оболочке (cmd, sh) можно установить переменные окружения, видимые только только для приложений запущенных в ней.

## Свойства приложения
В языке Java, помимо свойств окружения, можно задать свойства приложения, &mdash; не путать с параметрами приложения. Свойства приложения являются по сути параметрами виртуальной машины Java. Чтобы передать свойство приложения нужно использовать синтаксис с префиксом _-Dname=value_.
Рассмотрим программу, которая выведет сначала аргументы программы, далее свойства окружения, и в конце, &mdash; свойства приложения.


```
import java.util.stream.Stream;

public class App
{
    public static void main( String[] args )
    {
        System.out.println("Program arguments:");
        Stream.of(args).forEach(System.out::println);

        System.out.println("Environment properties:");
        System.getenv().entrySet().stream().forEach(System.out::println);

        System.out.println("Application properties:");
        System.getProperties().entrySet().forEach(System.out::println);
    }
}
```
Скомпилируем ее и запустим с параметрами:

```
javac App.java
java -Dmy.app.property=some_value App arg1 arg2
```
Возможный вывод программы:
```
Program arguments:
arg1
arg2

Environment properties:
LOCALAPPDATA=C:\Users\Администратор\AppData\Local
PROCESSOR_LEVEL=6
JAVA_HOME=C:\Program Files\Java\jdk-17.0.5
PROCESSOR_ARCHITECTURE=AMD64
USERNAME=Администратор

Application properties:
java.specification.version=17
os.name=Windows 10
my.app.property=some_value
```