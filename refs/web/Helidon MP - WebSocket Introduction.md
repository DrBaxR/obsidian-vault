Tags: #reference 
Created: 2022-09-05 12:09

# Helidon MP - WebSocket Introduction
This describes how you can use [[Helidon MP]] with [[WebSocket]]s. It uses Tyrus to provide support for the [[Jakarta EE]] websocket [[API]].

The websocket API allows [[Java]] application participate in websocket interactions as both servers and clients. The API allows two ways of using it: annotated and programatic endpoints.

## Example
A simple application that uses a [[RESTful API]] resource to push messages in a shared queue and a [[WebSocket]] endpoint to download messages from the queue one at a time over a connection.

REST endpoint implmented as [[JAX-RS]] resource and the shared queue (application scoped)

```java 
@Path("rest")  
public class MessageQueueResource {  
  
    @Inject  
    private MessageQueue messageQueue;  
  
    @POST  
    @Consumes("text/plain")  
    public void push(String s) {  
        messageQueue.push(s);  
    }  
}
```

The websocket endpoint with the annotated style

```java
@ServerEndpoint(  
    value = "/websocket",  
    encoders = { UppercaseEncoder.class })  
public class MessageBoardEndpoint {  
  
    @Inject  
    private MessageQueue messageQueue;  
  
    @OnMessage  
    public void onMessage(Session session, String message) {  
        if (message.equals("SEND")) {  
            while (!messageQueue.isEmpty()) {  
                session.getBasicRemote().sendObject(messageQueue.pop());  
            }  
        }  
    }  
}
```

`MessageBoardEndpoint` is just a [[POJO]], so it uses additional anotations for event handlers, such as `@OnMessage`.

The only things left that are needed to run the application are the supporting classes `MessageQueue` and `UppercaseEncoder`.

By default, JAX-RS resources and websocket endpoints will be available under the root `/` path. This can be overridden for websockets like this ([[Helidon]] annotation used)

```java
@ApplicationScoped  
@RoutingPath("/web")  
public class MessageBoardApplication implements ServerApplicationConfig {  
    @Override  
    public Set<ServerEndpointConfig> getEndpointConfigs(  
            Set<Class<? extends Endpoint>> endpoints) {  
        assert endpoints.isEmpty();  
        return Collections.emptySet();      // No programmatic endpoints  
    }  
  
    @Override  
    public Set<Class<?>> getAnnotatedEndpointClasses(Set<Class<?>> endpoints) {  
        return endpoints;       // Returned scanned endpoints  
    }  
}
```

Now the root path for websocket endpoints will be `/web`. **Note:** `@RoutingPath` is not bean defining so you also need to use `@ApplicationScoped`.

## Resources
https://helidon.io/docs/v3/#/mp/websocket