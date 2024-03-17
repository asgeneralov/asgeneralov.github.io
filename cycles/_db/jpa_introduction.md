---
title: Введение в JPA
layout: default
order: 1
---
# {{ page.title}}
Центральным понятием JPA является PersitenceContext, который содержит набор уникальных сущностей и отвечает за управление их жизненным циклом. За то какие классы могут управляться контекстом отвечает PersistenceUnit, а операциии над сущностями выполняются через EntityManager, -- простой, не потокобезопасный интерфейс, который позволяет выполнять различные операции над PersistnсeCоntext.

Жизненный цикл PersistenceContext может быть ограничен транзакцией (transaction-scoped peristence context) или выходить за ее пределы (extended persistence context). Он определяется при создании управляемого контейнером (например, Spring Framework) EntityManager.

```
package jakarta.persistence;
public enum PersistenceContextType {
    TRANSACTION,
    EXTENDED
}
```
По умолчанию для управляемого контейнером EntityManager создается transaction-scoped PersistenceContext, поэтому все ресурсы связанные с контекстом освобождаютсыя сразу жу после выполнения транзакции. В случае расширенного контекста, он существует пока его принудительно не закроет контейнер приложзения и может распространяться на несколько транзакций и даже поддердивает выполнение операций над контекстом в отсутствие активной транзакции.



The persist, merge, remove, and refresh methods must be invoked within a transaction context when an
entity manager with a transaction-scoped persistence context is used. If there is no transaction context,
the jakarta.persistence.TransactionRequiredException is thrown.

Methods that specify a lock mode other than LockModeType.NONE must be invoked within a
transaction. If there is no transaction or if the entity manager has not been joined to the transaction,
the jakarta.persistence.TransactionRequiredException is thrown.

The find method (provided it is invoked without a lock or invoked with LockModeType.NONE) and the
getReference method are not required to be invoked within a transaction. If an entity manager with
transaction-scoped persistence context is in use, the resulting entities will be detached; if an entity
manager with an extended persistence context is used, they will be managed. See Section 3.3 for entity
manager use outside a transaction.




<a target="blank" href="https://jakarta.ee/specifications/persistence/3.1/" >Jakarta Persistence 3.1</a>
<a target="blank" href="https://github.com/spring-projects/spring-framework/issues/15076" >https://github.com/spring-projects/spring-framework/issues/15076</a>
---
<a target="blank" href="https://github.com/spring-projects/spring-framework/issues/15076" >https://github.com/spring-projects/spring-framework/issues/15076</a>
<a target="blank" href="https://stackoverflow.com/questions/31335211/autowired-vs-persistencecontext-for-entitymanager-bean" >https://stackoverflow.com/questions/31335211/autowired-vs-persistencecontext-for-entitymanager-bean</a>
<a target="blank" href="https://stackoverflow.com/questions/75699120/jpa-entity-manager-transaction-scope-vs-extended-scope" >JPA Entity Manager transaction scope vs extended scope</a>