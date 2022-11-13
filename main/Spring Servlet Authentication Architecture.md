Tags: #backend  #spring #security 
Created: 2022-11-12 18:11
References: https://docs.spring.io/spring-security/reference/servlet/authentication/architecture.html

# Spring Servlet Authentication Architecture
This note can be considered a continuation of [[Spring Security Architecture]], that describes the main architectural components of [[Spring Security]] used in [[Servlet]] authentication.

## SecurityContextHolder
`SecurityContextHolder` is what contains the `SecurityContext`. This is where spring stores who is authenticated. Spring does not care how it is populated: if it contains a value, that value is used as the authenticated user.

## SecurityContext
`SecurityContext` contains an `Authentication` object.

## Authentication
It serves two main pusposes:
- an input to `AuthenticationManager` to provide creadentials a user provided to authenticate
- represents the currently authenticated user

`Authentication` contains:
- `principal` - identifies the user
- `credentials` - in many cases it is a password
- `authorities` - high level permissions the user is granted (eg. roles or scopes)

## GrantedAuthority
High level permissions the user is granted.

Each `Authentication` has a collection of `GrantedAuthority`s These roles are later configured for authorization; other parts of spring security are able to interpret these authorities.

## AuthenticationManager
It is an API that defines how spring's `Filter`s perform authentication. The `Authentication` that is returned by it is then set on the `SecurityContextHolder` by the controller that invoked the authentication manager.

## ProviderManager
The most commonly used implementation of `AuthenticationManager`.

It delegates a list of `AuthenticationProviders`, each of which being able to indicate that autheitication is successful, failed or indicate that it cannot decide.

In practice, each `AuthenticationProvider` is capable of doing a specific type of authentication.

## AuthenticationProvider
Multiple of these can be injected in a `ProviderManager`. Each of them can perform a specific type of authentication.

**Example:**  `DaoAuthenticationProvider` supports username/password authentication, while `JwtAuthenticationProvider` supports authenticating a [[JWT]].

## AuthenticationEntryPoint
This is used to send an [[HTTP]] response that requests credentials from a client.

Sometimes, a client proactively includes credentials to request a resource, cases in which there is no need for this.

In other cases a client sends an unauthenticated request to a resource, cases in which an implementation of `AuthenticationEntryPoint` is used to request credentials from that client. This can be done in many ways, some of which being redirecting to a login page, responding with a [[WWW-Authenticate]] header etc.

## AbstractAuthenticationProcessingFilter
This is used as a base filter (as in, the filter that inserter in the `SecurityFilterChain`) for authenticating the user's credentials. This happens after the credentials are requested by `AuthenticationEntryPoint`.

The authentication process looks something like this:
1. When the user submits credentials, the `AbstractAuthenticationProcessingFilter` creates an `Authentication` from the request that is to be authenticated.
2. `Authenticaton` is passed to the `AuthenticationManager` to be authenticated.
3. If authentication fails, then failure procedure.
4. If authentication succeeds, then success procedure.