---
layout: default
title:  "Добавление индекса в партиционированную таблицу Postrges"
---
# {{ page.title}}
Представим, что у нас есть табличка "person", которая разбита на партиции по континентам:

```
create table person (
    name varchar(200),
    continent varchar(200),
    code int not null
) PARTITION BY LIST (continent);

create table person_africa partition of person for values in ('Africa');
create table person_eurasia partition of person  for values in ('Eurasia');
create table person_antarctica partition of person  for values in ('Antarctica');

insert into person values ('Vasya', 'Africa', 1)
                          ('Petya', 'Africa', 2),
                          ('Masha', 'Eurasia', 3),
                          ('Vlad', 'Eurasia',  4),
                          ('Chan', 'Antarctica', 5),
                          ('Ilyas', 'Antarctica', 6);

 name  | continent  | registration_stamp  | code
-------+------------+---------------------+------
 Vasya | Africa     | 2000-02-28 06:00:00 |    1
 Petya | Africa     | 2000-02-28 06:00:00 |    2
 Chan  | Antarctica | 2008-09-11 07:00:00 |    5
 Ilyas | Antarctica | 2009-12-03 01:00:00 |    6
 Masha | Eurasia    | 2001-03-13 12:00:00 |    3
 Vlad  | Eurasia    | 2001-06-09 03:00:00 |    4
 Vasya | Eurasia    | 2000-02-29 07:00:00 |    1
```

Теперь нам пришло в голову сделать уникальный индекс по колонке "code", чтобы ускорить поиск данных.

```
create unique index person_code_unique_idx on person (code);
ERROR:  unique constraint on partitioned table must include all partitioning columns
DETAIL:  UNIQUE constraint on table "person" lacks column "continent" which is part of the partition key.
```
При попытке создать уникальный индекс по колонке "code", мы получаем сообщение об ошибке. Ошибка говорит о том, что уникальный индекс должен включать в себя все колонки ключа партиционирования. В нашем случае, это колонка "continent".

Попробуем создать индекс с двумя колонками и посмотрим, будет ли он соответствовать нашим требованиям.

```
create index person_continent_code_unique_idx on person (code);
CREATE INDEX

insert into person values ('Vasya', 'Eurasia', '2000-02-29 07:00:00', 1);
INSERT 0 1
```
Теперь успешно удалось создать индекс. Однако, мы смогли добавить в таблицу еще одну строку с кодом "1", что означает, что наш новый индекс не такой, как мы изначально хотели - он уникален только в пределах одного континента и может повторяться в таблице.