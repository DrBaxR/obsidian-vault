---
tags:
 - backend
 - java
---

[[Helidon SE]] is a [[Microframework]] that features three core components required to create a [[Microservice]]: a web server, configuration and security.

## Web Server Component
The web server component is an asynchronous and reactive API that has a functional style. Here's an example of starting a web server with it:

```java
WebServer.create(
	Routing.builder()
		.get("/greet", (req, res) -> 
			res.send("Hello World!")
		)
	.build()
).start();
```

The `WebServer` interface provides basic server lifecycle and monitoring enhanced by configuration, routing, error handling, and building metrics and health endpoints.

## Configuration Component
The configuration component loads and processes configuration properties from a defined properties file (generally `application.yaml`). Here's an example of a config file:

```yaml
app:  
  greeting: "Greetings from the web server!"  
  
server:  
  port: 8080  
  host: 0.0.0.0  
  
security:  
  config:  
    require-encryption: false  
  
  providers:  
    - http-basic-auth:  
        realm: "helidon"  
        users:  
          - login: "ben"  
            password: "${CLEAR=password}"  
            roles: ["user", "admin"]  
          - login: "mike"  
            password: "${CLEAR=password}"  
            roles: ["user"]  
    - http-digest-auth:
```

In order to access this file's config properties, we can use [[Helidon]]'s `Config` interface. Here's the method that would start the server using the specified properties:

```java
private static void startServer() throws Exception {  
    Config config = Config.create();  
  
    Routing routing = Routing.builder()  
        .any((request, response) -> response.send(config.get("app.greeting").asString().get() + "\n"))  
        .build();  
  
    WebServer webServer = WebServer  
        .create(routing, config.get("server"))  
        .start()  
        .toCompletableFuture()  
        .get(10, TimeUnit.SECONDS);  
  
    System.out.println("INFO: Server started at: http://localhost:" + webServer.port() + "\n");  
}
```

## Security Component
Provides following features:
 - [[Authentication]]
 - [[Authorization]]
 - Outbound security
 - Audit

The security component supports 3 different approaches: builder pattern, configuration pattern and a hybrid mode (for more in depth examples, check second resource).

To create a security component using the _configuration pattern_, you can do the following:

Java file
```java
// uses io.helidon.Config
Security security = Security.create(config);
```

application.yaml
```yaml 
# Uses config encryption filter to encrypt passwords  
security:  
providers:  
  - abac:  
  - http-basic-auth:  
		realm: "helidon"  
		users:  
		  - login: "jack"  
			password: "${CLEAR=password}"  
			roles: ["user", "admin"]  
		  - login: "jill"  
			password: "${CLEAR=password}"  
			roles: ["user"]
```

### Resources
https://www.infoq.com/articles/helidon-tutorial/
https://helidon.io/docs/v2/#/se/security/01_introduction