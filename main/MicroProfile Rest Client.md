Tags: #backend #microprofile
Created: 2022-09-03 22:09
References: [[Rest Client for MicroProfile]]

# MicroProfile Rest Client
Helps you send [[HTTP]] requests from [[MicroProfile]]. The way you do it is you create an interface which describes the methods that you can do to an [[API]] and then you can inject that same interface to use the client.

```java
@RegisterRestClient(baseUri="http://someHost/someContextRoot")
public interface MyServiceClient {
    @GET
    @Path("/greet")
    Response greet();
}
```

```java
@ApplicationScoped
public class MyService {
    @Inject
    @RestClient
    private MyServiceClient client;
}
```