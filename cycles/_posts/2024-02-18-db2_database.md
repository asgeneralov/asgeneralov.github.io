---
layout: default
title:  "H2 в памяти. Неожиданное удаление таблиц"
---
# {{ page.title}}

В Spring базу данных H2 можно сконфигурировать при помощи следующего кода:

Config.java
```
import org.springframework.boot.jdbc.DataSourceBuilder;
import org.h2.jdbcx.JdbcDataSource;

@Bean
@ConfigurationProperties(prefix = "data-source-1")
public DataSource dataSource() {
    return DataSourceBuilder.create()
            .type(JdbcDataSource.class)
            .build();
}
```
application.yaml
```
data-source-1:
  url: jdbc:h2:mem:mydb
  user: sa
  password: password
  driver-class-name: org.h2.Driver
```
Однако, при этом постоянно возникает ситуация, что в БД что-то записали, а потом найти этого не получилось.  Оказывается, база данных H2, в режиме работы в пямяти, создается каждый раз заново при новом JDBC соединении и удаляется при закрытии соединения. Вот такой вот прикол. 

Для того чтобы предотвратить удаление данных, нужно передать в настройках JDBC соединения параметр DB_CLOSE_DELAY=-1
```
data-source-1:
  url: jdbc:h2:mem:mydb;DB_CLOSE_DELAY=-1
```

## На заметку
В Spring существует специальный вспомогательный класс EmbeddedDatabaseBuilder помогающий конфигурировать подключения к встраиваемым БД. Строитель сам позаботится о нюансах создания соединения для популярных БД, и вам даже не потребуется что-то прописывать в applicationl.yaml.

Пример создания подключения в БД H2 в памяти при помощи EmbeddedDatabaseBuilder:
```
org.springframework.jdbc.datasource.embedded
@Bean
public DataSource dataSource() {
    return new EmbeddedDatabaseBuilder()
                .setType(EmbeddedDatabaseType.H2)
                .build();
}
```


## Ссылки
- <a target="blank" href="https://stackoverflow.com/questions/5763747/h2-in-memory-database-table-not-found/5936988#5936988" >H2 in-memory database. Table not found</a>
- <a target="blank" href="https://www.h2database.com/html/commands.html?highlight=DB_CLOSE_DELAY&search=DB_CLOSE_DELAY#set_db_close_delay" >SET DB_CLOSE_DELAY</a>

