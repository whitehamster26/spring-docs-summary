# IoC Continer

## Что это такое

IoC Container (Контейнер для внедрения зависимостей) - это главная сущность Spring Framework.  
Spring Framework имеет сообственный AOP (Aspect-Oriented-Programming) фреймворк, который удовлетворяет большинству требований для корпоративной разработки.  
IoC Container полностью охвачен AOP технологиями Spring Framework.  

Принцип IoC также известен как принцип внедрения зависимостей (DI - Dependency Injection).  
Это процесс, при котором объекты объявляют их зависимости (объекты, с которыми они работают) через аргументы конструктора, аргументы, которые указывают на фабричный метод, или поля (properties), которые указывают на экземпляр объекта после создания.  
IoC Container внедряет эти зависимости когда создает бин (Bean).

Основой IoC Контейнера являются пакеты `org.springframework.beans` и `org.springframework.context`. Интерфейс [BeanFactory](https://docs.spring.io/spring-framework/docs/5.3.10/javadoc-api/org/springframework/beans/factory/BeanFactory.html) предоставляет механизм конфигурации, подходящий для управления любым типом объектов. `ApplicationContext` является субинтерфейсом `BeanFactory`. Его функции:
* Способствует более легкой интеграции со Spring AOP
* Поддерживает Message resource handling для интернациализации (см. [MessageSource](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/MessageSource.html)) [пример](https://zetcode.com/spring/messagesource/))
* Поддерживает события ([пример](https://www.baeldung.com/spring-events))
* Поддерживает специализированные контексты уровня приложений, например `WebApplicationContext`

В общем, `BeanFactory` предоставляет базовую функциональность, а `ApplicationContext` добавляет более специализированную функциональность.

### Сравнение BeanFactory и ApplicationContext

| Feature | BeanFactory | ApplicationContext
|--- |--- |---
| Создание/связывание бинов | + | +
| Управление жизненным циклом бинов | - | +
| Автоматическая регистрация `BeanPostProcessor` | - | +
| Автоматическая регистрация `BeanFactoryPostProcessor` | - | +
| Доступ к `MessageSource` | - | +
| Встроенный механизм управления событиями (`ApplicationEvent`) | - | +

## Описание Контейнера, конфигурация.

Интерфейс `org.springframework.context.ApplicationContext` представляет IoC Container, а так же берет ответственность за создание, конфигурацию и сборку Бинов.  
Контейнер берет инструкции по созданию, управлению бинами, используя метаданные. Метаданные позволяют описать объекты, которые составляют ваше приложение и их связь.  
Метаданные могут быть представлены:

* С помощью XML (`XML-Based Configuration`)
* В Java аннотациях (`Annotation-Based Configuration` @Since Spring 2.5)
* В Java коде. (`Java-Based Configuration` @Since Spring 3.0)

Примеры всех трех типов конфигурации https://www.baeldung.com/spring-application-context  

Конфигурация содержит как минимум одно, а обычно более одного определения Бина, которым контейнер должен управлять.  

### Определение бина в конфигурациях

* ***XML-Based Configuration*** - `<bean/>` элемент внутри тэга `<beans/>`
* ***Java-Based Configuration*** - Методы, помеченные аннотацией `@Bean` внутри класса, помеченного аннотацией `@Configuration`