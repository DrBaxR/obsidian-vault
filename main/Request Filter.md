Tags: #backend 
Created: 2022-09-01 15:09
References: [[Jersey Filters and Interceptors]]

# Request Filter
Request filters are on type of [[Jersey Filters]] that can be implemented by doing something like this

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