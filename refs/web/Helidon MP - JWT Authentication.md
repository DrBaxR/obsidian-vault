Tags: #reference 
Created: 2022-08-24 14:08

# Helidon MP - JWT Authentication
## Usage
In order to use [[Helidon MP]]'s [[JWT]] [[Authentication]], you first need to add this dependency in the `pom.xml`:
```xml
<dependency>
	<groupId>io.helidon.microprofile.jwt</groupId>
	<artifactId>helidon-microprofile-jwt-auth</artifactId>
</dependency>
```

The minimal requires setup for a [[JAX-RS]] application is done by using the `@LoginConfig(authMethod="MP_JWT")`.

## API
- `JsonWebToken` - interface used in `@RequestScoped` beans to obtain the [[JWT]] of the currently executing request
- `@Claim` - used by `@ReuqestScoped` bean to obtain individual [[Claim]]s from the caller's JWT
- `ClaimValue` - interface used with `@Claim` to obtain the value of a claim by calling `getValue()`

## Resources
https://helidon.io/docs/v2/#/mp/jwtauth/01_introduction