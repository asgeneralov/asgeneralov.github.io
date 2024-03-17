---
layout: default
title:  "Нюансы с получением EntityManager в Spring"
---
# {{ page.title}}
### Рассмотрим попытку сконфигурировать EntityManager следующим образом:
```
@Primary
@Bean
public EntityManager entityManager(EntityManagerFactory entityManagerFactory) {
    return entityManagerFactory.createEntityManager();
}
```
Далее мы его используем так:
```
@Component
public class ServiceWithTransactionalMethod {
    
    @Autowired
    private EntityManager emAw;

    @Transactional
    public void methodTransactionalEmAutowired(Long id) {
        Person p = emAw.find(Person.class, id);
        p.setName(p.getName() + SUFFIX);
    }

    public void methodNonTransactionalEmAutowired(Long id) {
        Person p =  emAw.find(Person.class, id);
        p.setId(id);
        p.setName(p.getName() + SUFFIX);
        emAw.merge(p);
    }
}
```
Напишем пару тестов:
```
@SpringBootTest(classes = App.class)
@TestInstance(TestInstance.Lifecycle.PER_CLASS)
public class ServiceWithTransactionalMethodTest {

    @Test
    public void testMethodTransactionalEmAw() {
        JdbcTemplate jdbcTemplate = new JdbcTemplate(dataSource);
        service.methodTransactionalEmAutowired(1L);
        Person person = jdbcTemplate.queryForObject("select id, name from person where id = 1", new PersonRowMapper());
        Assertions.assertNotEquals("Vasya" + SUFFIX, person.getName());
    }

    @Test
    public void testMethodNonTransactionalEmAw() {
        JdbcTemplate jdbcTemplate = new JdbcTemplate(dataSource);
        service.methodNonTransactionalEmAutowired(4L);
        Person person = jdbcTemplate.queryForObject("select id, name from person where id = 4", new PersonRowMapper());
        Assertions.assertNotEquals("Nina" + SUFFIX, person.getName());
    }

}

``` 
### Запускаем тест с именем testMethodTransactionalEmAw

На методе methodTransactionalEmAutowired стоит аннотация @Transactional, он отрабатывает без ошибок, 
 но в базе изменения не появляются. 
### Запускаем теперь тест testMethodNonTransactionalEmAw

Он отрабатывает без ошибок, что странно, т.к. внутри em.persist(), который требует наличие транзакции. В базе изменения не появились.

### Добавим теперь еще один EntityManager, но другим способом:

```
@Component
public class ServiceWithTransactionalMethod {

    ...

    @PersistenceContext
    private EntityManager emPc;

    public void printEmClasses() {
        System.out.println("Em persistence context :"  + emPc.getClass());
        System.out.println("Em autowired :"  + emAw.getClass());
    }

    @Transactional
    public void methodTransactionalEmPersistenceContext(Long id) {
        Person p = emPc.find(Person.class, id);
        p.setName(p.getName() + SUFFIX);
    }

    public void methodNonTransactionalEmPersistenceContext(Long id) {
        Person p =  emAw.find(Person.class, id);
        p.setId(id);
        p.setName(p.getName() + SUFFIX);
        emPc.merge(p);
    }

}
```
### А так же пару аналогичных тестов:
```

@SpringBootTest(classes = App.class)
@TestInstance(TestInstance.Lifecycle.PER_CLASS)
public class ServiceWithTransactionalMethodTest {

...

    @Test
    public void printClasses() {
        service.printEmClasses();
    }

    @Test
    public void testMethodTransactionalEmPc() {
        JdbcTemplate jdbcTemplate = new JdbcTemplate(dataSource);
        service.methodTransactionalEmPersistenceContext(2L);
        Person person = jdbcTemplate.queryForObject("select id, name from person where id = 2", new PersonRowMapper());
        Assertions.assertEquals("Petya" + SUFFIX, person.getName());
    }

    @Test
    public void testMethodNonTransactionalEmPc() {
        JdbcTemplate jdbcTemplate = new JdbcTemplate(dataSource);
        Assertions.assertThrowsExactly(TransactionRequiredException.class, () -> {
            service.methodNonTransactionalEmPersistenceContext(3L);
        });
        Person person = jdbcTemplate.queryForObject("select id, name from person where id = 3", new PersonRowMapper());
        Assertions.assertNotEquals("Masha" + SUFFIX, person.getName());
    }

}

```
### Тест с именем testMethodTransactionalEmPc 
@Transactional в этот раз отработал штатно и изменения внесённые в сущность Peron сохранились в базу данных. 
### Текст с именем testMethodNonTransactionalEmPc 
При попытке сделать persist в методе не помеченном @Transactional, кидается исключение TransactionRequiredException.
### Так в чем разница?

Посмотрим на вывод теста printClasses:
```
Em persistence context :Shared EntityManager
Em autowired :SessionImpl(1936269454<open>)
```
То есть в случае @Autowired, мы получаем каждый раз один и тот же экземпляр EntityManger в виде реализации SessionImpl, 
который мы создали в конфигурационном методе  

```
@Primary
@Bean
public EntityManager entityManager(EntityManagerFactory entityManagerFactory) {
    return entityManagerFactory.createEntityManager();
}
```

Такой EntityManager не получает интеграцию с декларативным управлением транзакций Spring, не сохраняет изменения в базу и даже не кидает ошибок. 
Кроме того, он не безопасен к использованию, если наш сервис или DAO могут быть использованны из разных потоков, т.к. EntityManager не является потокобезопасным классом.
С другой стороны, EntityManager полученный через @PersistenceContext, вставляет вместо себя SharedEntityManager прокси, 
который транслирует вызовы к разным EntytyManager в зависимости от контекста использования, например, от потока который его вызывает, а так же получает интеграцию
с декларативным управлением транзакциями Spring.

У аннотации @Autowired, тем не менее, есть преимущество по сравнению с @PeristenceContext, состоящее в том, что в отличие от последней, она может быть использована
в т.ч. в конструкторах и в параметрах методов. Если это является существенным недостатком для вас, то исправить ситуацию можно создав EntityManager следующим образом:
```
@Bean
public EntityManager sharedEntityManager(EntityManagerFactory emf) {
    return SharedEntityManagerCreator.createSharedEntityManager(emf);
}
```
Тогда мы получим, ровно то, что и при получени EntityManager через @PersistenceContext, но только через @Autowired.



- <a target="blank" href="https://github.com/asgeneralov/cases/tree/main/data/jpa/em/em-injection" >Исходный код примеров</a>
- <a target="blank" href="https://jakarta.ee/specifications/persistence/3.1/" >Jakarta Persistence 3.1</a>
- <a target="blank" href="https://github.com/spring-projects/spring-framework/issues/15076" >Spring Framework Issue on GitHub</a>


