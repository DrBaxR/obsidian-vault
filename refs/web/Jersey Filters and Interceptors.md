Tags: #reference 
Created: 2022-08-31 16:08

# [[Jersey]] Filters and Interceptors
Suppose we have a [[JAX-RS]] application with a bunch of paths.

## Filters
[[Jersey Filters]] let you modify the properties of requests and responses. They are **always** executed, regardless of whether the resource was found or not.

In order to create a [[Request Filter]], you can implement the `ContainerRequestFilter` and registering it as a provider:

```java
@Provider
public class RestrictedOperationsRequestFilter implements ContainerRequestFilter { 

	@Override 
	public void filter(ContainerRequestContext ctx) throws IOException {
		if (ctx.getLanguage() != null && "EN".equals(ctx.getLanguage() .getLanguage())) {
			ctx.abortWith(Response.status(Response.Status.FORBIDDEN)
				.entity("Cannot access")
				.build()); 
		} 
	} 
}
```

**Note:** This filter is executed *after* the resource was matched. To execute a filter before the resource was matched, use a pre-matching filter using `@PreMatching`

```java
@Provider  
@PreMatching  
public class PrematchingRequestFilter implements ContainerRequestFilter {  
  
    @Override  
    public void filter(ContainerRequestContext ctx) throws IOException {  
        if (ctx.getMethod().equals("DELETE")) {  
            LOG.info("\"Deleting request");  
        }  
    }  
}
```

Implementing a [[Response Filter]] is really similar, only difference being the interface that needs to be implemented. On this filter the first parameter (`ContainerRequestContext`)

```java
@Provider  
public class ResponseServerFilter implements ContainerResponseFilter {  
  
    @Override  
    public void filter(ContainerRequestContext requestContext,  
        ContainerResponseContext responseContext) throws IOException {  
        responseContext.getHeaders().add("X-Test", "Filter test");  
    }  
}
```

## Interceptors
[[Jersey Interceptors]] are more connected with the marshalling and unmarshalling of the [[HTTP]] message bodies that are contained in the requests and responses.

This is the starting code (more on `createClient` later):
```java
@POST
@Path("/custom")
public static Response getCustomGreeting() {  
    return createClient().target(URI_GREETINGS + "/custom")  
        .request()  
        .post(Entity.text("custom"));  
}
```

**Reader interceptors** allow us to manipulate inbound streams (so the request on the server side). Here is an interceptor that adds a message to the body of the intercepted request:

```java
@Provider  
public class RequestServerReaderInterceptor implements ReaderInterceptor {  
  
    @Override  
    public Object aroundReadFrom(ReaderInterceptorContext context)  
    throws IOException, WebApplicationException {  
        InputStream is = context.getInputStream();  
        String body = new BufferedReader(new InputStreamReader(is)).lines()  
        .collect(Collectors.joining("\n"));  
  
        context.setInputStream(new ByteArrayInputStream(  
                (body + " message added in server reader interceptor").getBytes()));  
  
        return context.proceed();  
    }  
}
```

**Note:** You have to call `proceed()` to call the next interceptor in the chain. Once all the interceptors are executed, the appropiate message body reader will be called.

**Writer interceptors** are the same concept, only they are applied on outbound streams.

```java
@Provider  
public class RequestClientWriterInterceptor implements WriterInterceptor {  
  
    @Override  
    public void aroundWriteTo(WriterInterceptorContext context)  
    throws IOException, WebApplicationException {  
        context.getOutputStream()  
            .write(("Message added in the writer interceptor in the client side").getBytes());  
  
        context.proceed();  
    }  
}
```

These interceptors also need to be **registered in the client configuration** (back to the `createClient` method).

```java
private static Client createClient() {  
    ClientConfig config = new ClientConfig();  
    config.register(RequestClientFilter.class);  
    config.register(RequestWriterInterceptor.class);  
  
    return ClientBuilder.newClient(config);  
}
```

## Execution order

Here is the order of execution of filters and interceptors (can ignore the client side).

![Execution order of filters and interceptors](https://www.baeldung.com/wp-content/uploads/2018/03/Jersey2.png)

When having several filters and interceptors, we can specify the exact order by annotating them with the `@Priority` annotation.

```java
@Provider  
@Priority(Priorities.AUTHORIZATION)  
public class RestrictedOperationsRequestFilter implements ContainerRequestFilter {  
    // ...  
}
```

## Name binding
So far all the filters and interceptors were *global*, because they're executed on every request and response. It is also possible to define them to **execute only for specific resource methods**, which is called [[Name Binding]]

### Static binding
Create a particular annotation that will be used in the desired resources.
1. Create the annotation and annotate it with `@NameBinding`

```java
@NameBinding  
@Retention(RetentionPolicy.RUNTIME)  
public @interface HelloBinding {  
}
```

2. Annotate the resources for which you want the filter to be executed with the annotation you created

```java
@GET  
@HelloBinding  
public String getHelloGreeting() {  
    return "hello";  
}
```

3. Annotate the filter with the annotation created

```java
@Provider  
@Priority(Priorities.AUTHORIZATION)  
@HelloBinding  
public class RestrictedOperationsRequestFilter implements ContainerRequestFilter {  
    // ...  
}
```

### Dynamic binding
Say we have this resource, which in contained in a class called `Greetings`

```java
@GET  
@Path("/hi")  
public String getHiGreeting() {  
    return "hi";  
}
```

Now, to create a binding you can implement the interface `DynamicFeature` where you can do something like this

```java
@Provider  
public class HelloDynamicBinding implements DynamicFeature {  
  
    @Override  
    public void configure(ResourceInfo resourceInfo, FeatureContext context) {  
        if (Greetings.class.equals(resourceInfo.getResourceClass())  
            && resourceInfo.getResourceMethod().getName().contains("HiGreeting")) {  
            context.register(ResponseServerFilter.class);  
        }  
    }  
}
```

**IMPORTANT:** You have to delete the `@Provider` annotation on the filter, otherwise it will get executed twice, since it will also be considered a global filter.

## Resources
https://www.baeldung.com/jersey-filters-interceptorsprivate