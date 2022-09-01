Tags: #backend 
Created: 2022-09-01 15:09
References: [[Jersey Filters and Interceptors]]

# Response Filter
A response filter is a type of [[Jersey Filters]] which you can implement by doing something like this

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