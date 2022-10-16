Tags: #reference #backend 
Created: 2022-10-16 10:10

# Spring IoC
The [[Spring]] Inversion of Control of Dependency Injection (DI) is a mecanism tat lets you not worry about instantiating objects.

The objects that form the backbone of your application and that are managed by the Spring IoC container are called [[Bean]]s.

## Container overview
The `ApplicationContext` interface represents the Spring IoC container and is responsible for instantiating, configuring and assembling the beans. This container can be configured with XML, [[Annotation]]s or code.

The overview of the system is something like this: *Your application classes are combined with configuration metadata so that, after the `ApplicationContext` is created and initialized, you have a fuly configured and executable system or application.*

### Configuration metadata
Configuration metadata is traditionally supplied (however it is not the only way you can provide it) in XML format which is what this chapter uses to convey key concepts.

This metadata contains things like what the beans of the application are and how they should be configured.

### Instantiating the container
You can provide the paths to the config files when instantiating `ApplicationContext`:

```java
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");
```

Here's an example of the `services.xml` and `daos.xml` configuration files:

*services.xml*
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- services -->

    <bean id="petStore" class="org.springframework.samples.jpetstore.services.PetStoreServiceImpl">
        <property name="accountDao" ref="accountDao"/>
        <property name="itemDao" ref="itemDao"/>
        <!-- additional collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions for services go here -->

</beans>
```

*daos.xml*
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="accountDao"
        class="org.springframework.samples.jpetstore.dao.jpa.JpaAccountDao">
        <!-- additional collaborators and configuration for this bean go here -->
    </bean>

    <bean id="itemDao" class="org.springframework.samples.jpetstore.dao.jpa.JpaItemDao">
        <!-- additional collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions for data access objects go here -->

</beans>
```

It is also possible to have your XML config span across multiple files, and you can also specify it using Groovy Bean Definition DSL.

### Using the container
`ApplicationContext` is the interface for a complex factory that can maintain a registry of different beans and their dependencies.

```java
// create and configure beans
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");

// retrieve configured instance
PetStoreService service = context.getBean("petStore", PetStoreService.class);

// use configured instance
List<String> userList = service.getUsernameList();
```

Ideally, your code should have no dependency on the Spring APIs at all, so not calls to the `getBean()` method, since the other parts of the framework deal with this.

## Dependencies
A typical application does not consist of a single object (or bean, in the Spring world), it has multiple objects that work together to achieve a goal. This section describes how you go from defining a number of bean definitions that stand alone to a fully realized application where objects collaborate.

### Dependency injection
DI is the process where objects define their dependencies and have the container then inject them when it creates the bean.

This is the **incerse** of the bean itseld controlling the instantiation or location of its dependencies using direct construction.

There are two major variants of dependency injection: *constructor-based* and *setter-based*.

## Bean scopes
You can control not only the dependencies and configuration values that are to be plugged into an object from a bean definition, but also control the **scope** of the created objects.

The Spring framework supports 6 scopes, 4 of which are available only if you use the web-aware `ApplicationContext`:
- singleton
- prototype
- request
- session
- application
- websocket

## Lifecycle callbacks
It's possible to specify some methods that get called by Spring after te bean was created/before it is destroyed.

The recommended way of doing this is by using the annotations `@PostConstruct` and `@PreDestroy`.

## Annotation-based container configuration
Instead of using XML to describe a bean wiring, it is possible to use [[Annotation]]s to move the configuration into the component class itself, by annotating the class, method or field declaration.

Note: Annotation injection is performed before XML injection, tuhs XML config overrides annotations for properties wired through both approaches.

## Using `@Value`
This annotation is used to inject externalized properties. Here's an example that injects a value from the `application.properties` file:

```java
@Component
public class MovieRecommender {

    private final String catalog;

    public MovieRecommender(@Value("${catalog.name}") String catalog) {
        this.catalog = catalog;
    }
}
```

*configuration:*
```java
@Configuration
@PropertySource("classpath:application.properties")
public class AppConfig { }
```

*application.properties*
```java
catalog.name=MovieCatalog
```

## JSR 330 Standard annotations
Apparently, there is a set of standard annotations that are preferred. These are provided by [[JSR 330]]  
(`javax.inject` package).

## Java based container configuration
I think this is the most important section so far, so in case you need more details I'll directly link it [here](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-java).

The central artifacts are `@Configuration` annotated classes and `@Bean` annotated methods. The latter indicates that a method instantiates, configures and initializes a new objects to be managed by IoC container. The former indicates that its primary purpose is to be a source of bean definitions.

## Environment abstraction
This lets you bring yout own configurations of the application that you are developing.

### Profiles
It's possible (and likely) that you want some config to be done for development environment and some other config for production environment. Here's where you can use the `@Profile` annotation on your beans.

```java
@Configuration
@Profile("development")
public class StandaloneDataConfig {

    @Bean
    public DataSource dataSource() {
        return new EmbeddedDatabaseBuilder()
            .setType(EmbeddedDatabaseType.HSQL)
            .addScript("classpath:com/bank/config/sql/schema.sql")
            .addScript("classpath:com/bank/config/sql/test-data.sql")
            .build();
    }
}
```

```java
@Configuration
@Profile("production")
public class JndiDataConfig {

    @Bean(destroyMethod="")
    public DataSource dataSource() throws Exception {
        Context ctx = new InitialContext();
        return (DataSource) ctx.lookup("java:comp/env/jdbc/datasource");
    }
}
```

## Resources
https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans