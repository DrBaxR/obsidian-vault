---
tags:
 - java
 - backend
---
# JAX-RS
JAX-RS is a specification that contains a set of interfaces and [[Annotation]]s offered by [[Jakarta EE]] used for the development of [[RESTful API]]s, that has many implementations, some of the most popular being RESTeasy and Jersey.

The fact that JAX-RS is only a specification helps _us_ (the _developers_) with the fact that the things that we need to deploy are very lightweight (since they only contain a couple of **interfaces** and **annotations**), leaving the "heavy lifting" (or the **implementations** of the interfaces) to be handled by the application server.

## Example
In order to start playing around with JAX-RS, the simplest way it to use [[Maven]] and include this (the interfaces of JAX-RS) dependency in the `pom.xml` file:

```xml
<dependency> 
	<groupId>javax</groupId> 
	<artifactId>javaee-api</artifactId> 
	<version>7.0</version> 
	<scope>provided</scope> 
</dependency>
```

The dependency specified above contains the [[API]] (interfaces and annotations) of JAX-RS and has its scope as `provided` because this [[JAR]] does not need to be in the final build, it is only needed at compile time.

The first thing that we need to do after that is create an `Application` class (extends a certain class and is annotated with a certain [[Annotation]]):

```java
@ApplicationPath("/api") 
public class RestApplication extends Application { }
```

The above sequence basically says that the entry point of the [[RESTful API]] that we create is the path `/api`.

Next up, we can create resources by annotating classes with the `@Path` annotation and the handler methods with the method (`@GET`, `@POST`, `@PUT` etc.) and path they respond to.

```java
@Path("/notifications")  
public class NotificationsResource {  
    @GET  
    @Path("/ping")  
    public Response ping() {  
        return Response.ok().entity("Service online").build();  
    }  
  
    @GET  
    @Path("/get/{id}")  
    @Produces(MediaType.APPLICATION_JSON)  
    public Response getNotification(@PathParam("id") int id) {  
        return Response.ok()  
            .entity(new Notification(id, "john", "test notification"))  
            .build();  
    }  
  
    @POST  
    @Path("/post/")  
    @Consumes(MediaType.APPLICATION_JSON)  
    @Produces(MediaType.APPLICATION_JSON)  
    public Response postNotification(Notification notification) {  
        return Response.status(201).entity(notification).build();  
    }  
}
```

In order to use this code, you need to build a [[WAR]] and deploy it to an application server (Tomcat, for example).

### Resources
https://www.baeldung.com/jax-rs-spec-and-implementations