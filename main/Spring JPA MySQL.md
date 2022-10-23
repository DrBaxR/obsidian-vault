Tags: #spring #backend 
Created: 2022-10-23 15:10
References: https://spring.io/guides/gs/accessing-data-mysql/

# Spring JPA MySQL
This note describes how you can use [[Spring]]'s [[JPA]] to with a [[MySQL]] database to persist data.

First of, you need to create a database. This note supposes that the database name is `data` and the user for the database has the credentials `root:password`.

The dependencies you need are as follow:

```xml
<!-- spring jpa -->
<dependency>  
   <groupId>org.springframework.boot</groupId>  
   <artifactId>spring-boot-starter-data-jpa</artifactId>  
</dependency>  
<dependency>  
   <groupId>org.springframework.boot</groupId>  
   <artifactId>spring-boot-starter-web</artifactId>  
</dependency>  

<!-- mysql driver -->
<dependency>  
   <groupId>mysql</groupId>  
   <artifactId>mysql-connector-java</artifactId>  
   <scope>runtime</scope>  
</dependency>  

<!-- sprong boot -->
<dependency>  
   <groupId>org.springframework.boot</groupId>  
   <artifactId>spring-boot-devtools</artifactId>  
   <scope>runtime</scope>  
   <optional>true</optional>  
</dependency>
```

After you have these dependencies, you need to configure spring by setting the following properties in the `application.properties` file:

```properties
spring.jpa.hibernate.ddl-auto=update  
spring.datasource.url=jdbc:mysql://${MYSQL_HOST:localhost}:3307/data  
spring.datasource.username=root  
spring.datasource.password=password  
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver  
#spring.jpa.show-sql: true
```

If you want to know more about the `hibernate.ddl-auto` property, you can read about it in the [[Hibernate]] [documentatiton](https://docs.jboss.org/hibernate/orm/5.4/userguide/html_single/Hibernate_User_Guide.html#configurations-hbmddl).

After you are done with the config, you need to create the `@Entity` model, which tells hibernate hwo to create the tables.

Other annotations that you might use are `@Id` for the id field of the entity and `@GeneratedValue(strategy = GenerationType.AUTO)`.

Next up, you need to create a repository for that entity, you can do this by creating an interface that extends `CrudRepository<T, ID>`, where T is the entity's type and ID is the type of the id of the entity.