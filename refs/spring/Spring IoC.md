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

## Resources
https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans