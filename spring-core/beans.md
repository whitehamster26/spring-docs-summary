# Beans

[Spring IoC Container](ioc-container.md) управляет сущностями, которые называются Бинами (Beans). Бины создаются с помощью метаданных, которые поставляются в контейнер.  

Внутри Контейнера, определения Бинов представлены как объекты [BeanDefinition](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/config/BeanDefinition.html), которые (среди прочей информации) содержат следующие метаданные: 
* Имя класса с указанием пакета ([getBeanClassName](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/config/BeanDefinition.html#getBeanClassName--))
* Описание поведения объекта в контейнере (Scope, Lifecycle callbacks и т.д.)
* Ссылки на другие объекты, которые необходимы для бина, чтобы он работал ([getDependsOn](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/config/BeanDefinition.html#getDependsOn--))
* Другие конфигурации, которые необходимо установить для нового объекта

Эти метаданные преобразуются в набор свойств, составляющих определение каждого Бина, такие как:

* Название класса
* Имя(айди) Бина
* Scope
* Аргументы конструктора
* Свойства (Properties)
* Autowiring mode
* Lazy initialization mode
* Initialization method
* Desctuction method
  
См. геттеры и сеттеры `BeanDefinition`

## Создание Бина

`BeanDefinition` является инструкцией для создания объектов. Контейнер смотрит на "инструкцию" для создания бина, а затем создает новый объект, согласно данным, опеределенным в `BeanDefinition`.  
Контейнер может создать Бин двумя способами:

* Вызов конструктора (Эквивалентно оператору `new`)
* Вызов статического Фабричного метода

### Жадность инициализации
  
  
По умолчанию `ApplicationContext` создает синглтоны (о скоупах далее) жадно, прямо во время инициализации.  
Это поведение можно переопределить, чтобы синглтоны создавались только тогда, когда к ним первый раз обращаются. Для этого в конфигурации надо указать 

 || Для метода | Для Класса
 |--|----------|-------------
XML | `<bean lazy-init="true">` | `<beans default-lazy-init="true">`
Annotations | `@Lazy(true)` | `@Lazy`

### Bean Scopes

Spring Framework поддерживает 6 скоупов, также можно создать свой собственный скоуп, имплементируя [org.springframework.beans.factory.config.Scope](https://docs.spring.io/spring-framework/docs/5.3.10/javadoc-api/org/springframework/beans/factory/config/Scope.html).  

#### Таблица скоупов
| Scope | Description | Lifecycle
|--- | --- | ---
singleton | Один `BeanDefinition` создает только один объект в IoC Контейнере | Время жизни `ApplicationContext` 
prototype | Один `BeanDefinition` создает сколько угодно объектов в Контейнере | IoC Контейнер не управляет этими объектами, время жизни неопределено.
request | Каждый HTTP запрос получает от `BeanDefinition` свой собственный экземпляр объекта | Один HTTP запрос
session | Каждая HTTP Сессия получает от `BeanDefinition` свой собственный экземпляр объекта | HTTP Сессия
application | Каждый `ServletContext` получает от `BeanDefinition` свой собственный экземпляр объекта | Время жизни `ServletContext`
websocket | Каждый `WebSocket` получает от `BeanDefinition` свой собственный экземпляр объекта | Время жизни `WebSocket`

## Управление жизненным циклом Бинов

Spring Framework использует имплементации [BeanPostProcessor](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/config/BeanPostProcessor.html) для управления всеми callback интерфейсами, которые ему встречаются.  

### Initialization Callbacks

Интерфейс [org.springframework.beans.factory.InitializingBean](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/InitializingBean.html) позволяет Бину выполнить какие-либо действия после того, как Бин будет сконфигурирован (все свойства будут установлены). Этот интерфейс содержит один метод:  
```java
 void afterPropertiesSet() throws Exception;
 ``` 

Чтобы не копировать код Спринга, рекомендуется использовать аннотацию `@PostConstruct`

### Destruction Callbacks

Интерфейс [org.springframework.beans.factory.DisposableBean](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/DisposableBean.html) позволяет Бину выполнить действия, когда контейнер, который управляет этим объектом, уничтожен. Этот интерфейс содержит один метод:
```java
void destroy() throws Exception;
```
Чтобы не копировать код Спринга, рекомендуется использовать аннотацию `@PreDestroy`

