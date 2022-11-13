Tags: #backend #spring #security 
Created: 2022-11-12 15:11
References: https://docs.spring.io/spring-security/reference/servlet/architecture.html

# Spring Security Architecture
This note describes the [[Spring]] security acthitecture.

## Filters
A spring application is made up out of a chain of filters that get executed everytime a request is received. They all happen in order between the moment a request is received and a response is formed.

Each of these filters can prevent the other downstream filters to get executed, by typically wirting the response; or modify the request and response that the downstream filters wil process.

Each of the filters in the chain have a structure that looks something like this:

```java
public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) {
	// do something before the rest of the application
    chain.doFilter(request, response); // invoke the rest of the application
    // do something after the rest of the application
}
```

**Note:** This is part of what basic Java [[Servlet]]s do.

## DelegatingFilterProxy
This is a filter that is implemented and registered by spring that delegates all its work to a [[Bean]]. This is the pseudocode of this special filter:

```java
public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) {
	// Lazily get Filter that was registered as a Spring Bean
	// For the example in DelegatingFilterProxy
delegate
 is an instance of Bean Filter0
	Filter delegate = getFilterBean(someBeanName);
	// delegate work to the Spring Bean
	delegate.doFilter(request, response);
}
```

## FilterChainProxy
This is a special filter provided by spring that allows delegating to multiple `Filter` instances through `SecurityFilterChain`. This `FilterChainProxy` is the bean that is wrapped in `DelegatingFilterProxy`.

## SecurityFilterChain
This is what is used to group a bunch of spring securitry `Filter`s.

There can me multiple `SecurityFilterChain`s that can be completely indelendently configured and then have each of them attributed to a certain type of requests, so the `Filter`s that they contain only get called for the requests that match the pattern assigned to the chain.

The decision of which `SecurityFilterChain` gets called for a request is made by `FilterChainProxy`. **Example:** You can have a filter chain assigned to all requets to the path `/api/**` and another filter assigned to the path `/**`.

## Security Filters
These are the `Filter`s that get inserted in a `SecurityFilterChain`. There are many of these filters that are already implemented by spring, some examples include: `CorsFilter`, `BearerTokenAuthenticationFilter`, `BasicAuthenticationFilter` and many more.

