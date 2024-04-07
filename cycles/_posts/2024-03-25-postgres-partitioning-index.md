---
layout: default
title:  "Добавление индекса в партиционированную таблицу Postrges"
---
# {{ page.title}}
Представим, что у нас есть табличка **person**, которая разбита на партиции по континентам:

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

Теперь нам пришло в голову сделать уникальный индекс по колонке **code**, чтобы ускорить поиск данных.

```
create unique index person_code_unique_idx on person (code);
ERROR:  unique constraint on partitioned table must include all partitioning columns
DETAIL:  UNIQUE constraint on table "person" lacks column "continent" which is part of the partition key.
```
При попытке сделать уникальный индекс по колонке **code** получаем сообщение об ошибке: уникальный индекс обязан содержать в себе все колонки ключа партиционирования, т. е. колонку continent в нашем случае. Попробуем создать индекс с двумя колонками, посмотрим, будет ли он отвечать нашим требованиям.

```
create index person_continent_code_unique_idx on person (code);
CREATE INDEX

insert into person values ('Vasya', 'Eurasia', '2000-02-29 07:00:00', 1);
INSERT 0 1
```
Теперь успешно получилось создать индекс. Однако мы смогли вставить в таблицу еще одну строку с кодом ***1***, а значит, наш новый индекс не то, что мы изначально хотели, — он уникален только в пределах одного континента и может повторяться в таблице.