---
layout: default
title:  "Индекс в партиционированных таблицах Postrges"
---
# {{ page.title}}

```
create table person (
    name varchar(200),
    continent varchar(200),
    registration_stamp timestamp,
    code int not null
) PARTITION BY LIST (continent);

create table person_africa partition of person for values in ('Africa');
create table person_eurasia partition of person  for values in ('Eurasia');
create table person_antarctica partition of person  for values in ('Antarctica');

insert into person values ('Vasya', 'Africa', '2000-02-28 06:00:00', 1),
                          ('Petya', 'Africa', '2000-02-28 06:00:00', 2),
                          ('Masha', 'Eurasia', '2001-03-13 12:00:00', 3),
                          ('Vlad', 'Eurasia', '2001-06-09 03:00:00', 4),
                          ('Chan', 'Antarctica', '2008-09-11 07:00:00', 5),
                          ('Ilyas', 'Antarctica', '2009-12-03 01:00:00', 6);

create unique index person_code_unique_idx on person (code);
ERROR:  unique constraint on partitioned table must include all partitioning columns
DETAIL:  UNIQUE constraint on table "person" lacks column "continent" which is part of the partition key.

create index person_continent_code_unique_idx on person (code);
CREATE INDEX

insert into person values ('Vasya', 'Eurasia', '2000-02-29 07:00:00', 1);
INSERT 0 1

Это не то что мы хотели, у нас code уникален только в пределах одного континента.

create index registration_stamp_idx on person (code);


postgres=# explain select * from person where registration_stamp > '2001-05-02';
 Append  (cost=0.00..3.10 rows=3 width=848)
   ->  Seq Scan on person_africa person_1  (cost=0.00..1.02 rows=1 width=848)
         Filter: (registration_stamp > '2001-05-02 00:00:00'::timestamp without time zone)
   ->  Seq Scan on person_antarctica person_2  (cost=0.00..1.02 rows=1 width=848)
         Filter: (registration_stamp > '2001-05-02 00:00:00'::timestamp without time zone)
   ->  Seq Scan on person_eurasia person_3  (cost=0.00..1.04 rows=1 width=848)
         Filter: (registration_stamp > '2001-05-02 00:00:00'::timestamp without time zone)

Почему не использутся индекс?








Africa

Antarctica

Europe

North America

South America

Asia

Australia

Oceania

Asia continent

```



