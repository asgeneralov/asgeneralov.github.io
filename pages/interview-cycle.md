---
title: Собеседование программист Java
layout: default
---
# {{ page.title}}


## Структуры данных J[+] M[+] S[-]
- Primitive data types
- Array
- Linked List
- Map
- Queue
- Deque

<div class="btn-toggle-answer btn-toggle-answer-state-hidden"><button>Показать ответ</button></div>

<div class="answer">
Ответ
</div>

Как устоены внутри различные реализации
ArrayList, LinkedList, HashMap

Map vs Collection 
Итерация и удаление элементов, Fail Fast, FIFO, LIFO
Что такое рекурсия


## Расширенные структуры данных J[-] M[+] S[+]
Tree
Self-balanced tree (red-black, B-tree)

## Базовые алгоритмы и алгоритмическая сложность J[-] M[-] S[-]
Big-O notation
Divide-and-conquer algorithms
Randomized algorithms
Quick sort
Merge sort
Greedly algorithms
Lists manipulations
Binary Search

# Java SE
## Методы класса Object, контракт между equals и hashCode J[+] M[+] S[-]
Метод toString(), где вызывается, нужно ли переопределять

## Примитивы, массивы, классы обертки для примитивов J[+] M[-] S[-]
Что аткле boxing / unboxing / autocasting

## Лямбды, method references, функции высшего порядка   J[-] M[+] S[+]
Чем отличаются промежуточные и терминальные операторы в Stream API?
Привести пример терминального и промежуточного оператора
В чем отличие flatMap() от map()
Что такое функциональный интерфейс, какие в Java есть функциональные интерфейсы


## try-catch-finally и try-with-resources  J[+] M[-] S[-]
Рассказать про классы исключениц, привести примеры исключений, checked и unchecked исключения
Всегда ли исполняется блок finally
Последовательность блоков catch, несколько исключений в одном блоке
Как бросить исключение, как определить что метод кидает исключениц

## Строки J[-] M[-] S[-]
Пулл строк

## Genrics  J[-] M[+] S[+]
Что такое
PECS, wildcart
Можно ли List<Number> some =  new ArrayList<Integer>() ?

# Java Language Fundamentals   J[+] M[-] S[-]
Понимание концепций ООП
Модификаторы достпа
protected vs default (package private)
Можно ли написать abstract static 
Применение интерфейсов и абстрактных классов
Перегрузка и переопределение методв
Можно ли переопределить статический метод

# Загрузчики классов  J[-] M[-] S[+]
Какие бывают загрузчики классов

# Многопоточность
synchronized method/block (static vs non static)  J[+] M[+] S[+]
Рассказать про захват монитора
volatile keyword  J[-] M[+] S[+]

Как создать новый тред, Runnable start() vs run()
Чем процесс отличается от потока
Что такое DeadLock

Пакет java.util.concurrent
Executor, Executor Service, Callable, Future
Standard Executors (Single Thread, Fixed, Pooled)

Механизм interappted в Java
В чем разница между interapted и isInterapted();

Atomic, Locks (ReentrantLock, ReentrantReadWriteLock)

Пакет java util concurrent, утилиты для синхронизации потоков
CountDownLatch, Semaphore, CyclicBarier, Exchanger

Concurrent Collections
CopyOnWriteArrayList/Set
ConcurrentSkipListMap/Set
ConcurrentHashMap

# JVM & Tools

Понимание основ GC
G1
Memory and Java Collecitons


# Spring Framework
Beans IoC Container 
Способы инжекта бинов в Spering
В чем отличие Spring Bean от классического 
Что такое Scope beanm, расказать про типы скоупов
Варианты реализации IoC
ЧТо если есть несколько претендентов на инжекшн, как эот решается
@Qualifier, @Primary
Что если не Sping не найдет кандидата, как решается
Этапы инийиализации бина, Bean Factory
Рассказать про Spring Application Events
Как запустить переодическую задачу с помощью спринга

Как сделать SpringCOntroller, что такое Dispatcher Servelt
Что такле Starter, как сделать starter?

# Design and development principles
Паттерны GOF
SOLID, KISS, GRASP, SOLID
Способы реализации Singleton
Примеры использования паттернов в реальной жизни
Proxy vs Wrapper vs Adapter

# Архиектура
Микросервисная архитектура
Что такое микросервис?
Распределенные системы. CAP теорема
Cloud Native

Контейнеризация
Docker, докерфайл, volumes, cgroups

Управление исходным кодом, модели ветвления git
В чем отличие fetch от pull
Что такое cherry-pick?
commit, push

# Системы сборки Maven
Понимание цикла сборки проекта и понятий Maven
(profile, dependency, repository)
Как создать новый маен проект 
Что делает clean install
package/install/deploy в чем разница
Зачем нужен pom
ЧТо такое секция dependencyManagement?
Что такое bill of material?
Какуие бывают scope в maven и их использование

# Database Development
ACID
Блокировки pessimistinc/optimistic
Как сделать пессимистическуюб блокировку? 
SELECT (FOR UPDATE, FOR KEY SHARE , FOR SHARE, FOR NO KEY UPDATE)
FOR UPDATE SKIP LOCKED
FOR UPDATE NOWAIT
(select for update NO WAIT)
На каком уровне работает оптимистическая блокировка (БД, приложения?)

# Тестирование
Пирамида тестирвоания. Инструметы тестирвоания Mockito, JUnit, 
TestContainers, Awailability

# Integration
Что такое сериализация/десериализация
Что такое идемподентность сервиса, Привести пример
Что таоке SQL injection;
Что такое Statefull и Stateless
XSD и WSDL 
Что такое xPath и XSLT
Что такое кеш, инвалидация кеша
Что такое eventually consistency
GRPC
Шаблон CIrcui Breaker
Что такое api gateway, service discovery, config server

# Web
Старуктура http запроса. Методы http?  В чем разница между POST и GET
Поддерживает ли HTTP состояние
Как сохранять состояние в HTTP
Что такое cookie, session id, sticky session  ?
Группы кодов http ответов
Чем отличается HTTP от HTTPS, стандартные порты HTTP и HTTPS

# SQL 
Чем having отличается от where
Для чего нужны join, виды join
Что такое constraint

# OpenShift
Что такое реплика
Чем отличается Deployment от DeploymentConfig
Что такое Liveness Probe и Rediness Probe
Какие классы ресурсов есть в OpenSHift
(Deployment, ConfigMap, Service, DestinationRule, Gateway, VirtualServie, EnvoyFilter)
Как выделяются ресурсы на сервисы? limit и  request
Что такое pod?

# Docker
Что такое Docker образ, что делаем docker run
DOckerfile сблорка образа
ЧТо такое volume
Что такое контейнер
Выделенеи памяти и ресурсов в образе















