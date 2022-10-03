Tags: #backend 
Created: 2022-10-01 15:10
References: https://helidon.io/docs/v3/#/mp/restclient

# Helidon REST Client
[[Helidon MP]] offers the possibility to send [[HTTP]] requests from the microservice that you are working via its rest client.

There are two ways you can use this module of the framework: programatically using a builder or via annotations using the [[CDI]] mechanism.

In order to use CDI, you first needto create an interface andannotate it with `@RegisterRestClient` gicing it the base URI of the API you want to call.

To add filters, interceptors or whatever to the client, you can use the `@RegistetProvider` annotaion on the interface.

```java
@RegisterRestClient(baseUri="http://localhost:8080") @RegisterProvider(GreetClientRequestFilter.class) @RegisterProvider(GreetClientExceptionMapper.class) 
public interface GreetRestClient { 
	// ... 
}
```

You can annotate the methods in the interface with the regular [[JAX-RS]] annotations, only difference being that you aredescribing the endpoints of the API that you want to call. Here's an example:

```java
@RegisterRestClient(baseUri="http://localhost:8080")
interface GreetRestClient { 

	@GET 
	JsonObject getDefaultMessage(); 
	
	@Path("/{name}") 
	@GET 
	JsonObject getMessage(@PathParam("name") String name); 
}
```

If the endpoints require certain headers for let's sat authentication for example, you can use the `@ClientHeaderParam` annotation and giveit either a hard coded value or a method that computes the value.s

```java
@Path("/api/verify")  
@RegisterRestClient  
@ClientHeaderParam(name = "Authorization", value = "{lookupAuth}")  
public interface VerifyEngine {  
  
	default String lookupAuth() {  
	return "Basic " +  
		Base64._getEncoder_().encodeToString("user:pass".getBytes());  
	}
}
```

