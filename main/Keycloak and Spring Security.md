Tags: #security #backend 
Created: 2023-03-07 11:03
References: https://medium.com/geekculture/using-keycloak-with-spring-boot-3-0-376fa9f60e0b 

# Keycloak and Spring Security
This note assumes that you have a Keycloak instance up and running, where you have created a realm called `realm` and a client called `client` in that realm.

## Keycloak Setup
First off, you need to create some client roles. Let's call them `admin` and `user`. After that, we can create some composite realm roles: `app_admin` and `app_user`, where `app_admin` contains the role `admin` and `app_user` contains the role `user`.

Now that we have created the roles, we can finally create some users to which we can assign whichever realm roles we please.

## Spring Setup
First thing that we need to do is add these dependencies in the `pom.xml`:

```xml
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-security</artifactId>  
</dependency>  
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-oauth2-resource-server</artifactId>  
</dependency>
```

The properties that are required by the OAuth 2.0 resource server dependency are as follows:

```yml
spring:
	security:  
	  oauth2:  
	    resourceserver:  
	      jwt:  
	        issuer-uri: http://...
	        jwk-set-uri: http://.../protocol/openid-connect/certs
```

After we took care of that, we can set up the application's security config. In order to do that we need to create a config class:

```kotlin
@Configuration  
@EnableWebSecurity  
@EnableMethodSecurity(  
    jsr250Enabled = true,  
)  
class WebSecurityConfig {  
  
    @Autowired  
    private lateinit var jwtAutoConverter: JwtAuthConverter  
  
    @Bean  
    fun securityFilterChain(http: HttpSecurity): SecurityFilterChain {  
        http.oauth2ResourceServer()  
            .jwt()  
            .jwtAuthenticationConverter(jwtAutoConverter)  
  
        http.sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)  
  
        return http.build()  
    }  
}
```

Note that here we set up out [[Security Filter Chain]] to use OAuth 2.0 resource server with [[JWT]]. We also specify the converter that transforms the JWT to an authentication token. Here is how that one looks like:

```kotlin
@Component  
class JwtAuthConverter(  
    val properties: JwtAuthConverterProperties  
) : Converter<Jwt, AbstractAuthenticationToken> {  
  
    private val jwtGrantedAuthoritiesConverter = JwtGrantedAuthoritiesConverter()  
  
    override fun convert(jwt: Jwt): AbstractAuthenticationToken? {  
        val authorities = Stream.concat(  
            jwtGrantedAuthoritiesConverter.convert(jwt)?.stream() ?: Stream.empty(),  
            extractResourceRoles(jwt)?.stream() ?: Stream.empty()  
        ).collect(Collectors.toSet())  
  
        return JwtAuthenticationToken(jwt, authorities, getPrincipalClaimName(jwt))  
    }  
  
    private fun getPrincipalClaimName(jwt: Jwt): String? {  
        val claimName = properties.principalAttribute ?: JwtClaimNames.SUB  
  
        return jwt.getClaim(claimName)  
    }  
  
    private fun extractResourceRoles(jwt: Jwt): Collection<GrantedAuthority>? {  
        val resourceAccess: Map<String, Any>? = jwt.getClaim("resource_access")  
        val resource = resourceAccess?.get(properties.resourceId) as Map<String, Any>? ?: return setOf()  
        val resourceRoles = resource["roles"] as Collection<String>? ?: return setOf()  
  
        return resourceRoles  
            .map { SimpleGrantedAuthority("ROLE_${it.uppercase()}") }  
            .toSet()  
    }  
}
```

This converter uses some properties that look like this:

```yaml
jwt:  
  auth:  
    converter:  
      resource-id: client
      principal-attribute: preferred_username
```

Here is what each of the properties mean:
- `jwt.auth.converter.resource-id`: the client scope
- `jwt.auth.converter.principal-attribute`: the field that will be used as the authentication token 

The properties are then injected in the converter by using a class that looks like this:

```kotlin
@Validated  
@Configuration  
@ConfigurationProperties(prefix = "jwt.auth.converter")  
class JwtAuthConverterProperties {  
  
    var resourceId: String? = null  
    var principalAttribute: String? = null  
}
```

## Ideas for Fine-Grained Access to Resources
By fine-grained access I mean something like this: You have a resource server that has users, employees and buildings (these are the resources); and you want to be able to restrict who can `create`, `read`, `update` and `delete` these resources.

### Attributes Approach
Each role contains attributes that specify what permissions the role has. The attributes are a string that contains a JSON array of predetermined keys. One of these arrays will be assigned for each type of action that can be executed.

For example, you can have a `user` role that will have the following attributes:
- `read: ["user", "employee", "building"]`
- `create: ["user"]`
- `update: ["user"]`
- `delete: []`

**Disadvantage:** The attributes are not contained in the JWT, therefore you will need to make a separate request for fetching a role's attributes.

### Composite Roles Approach
The first idea I got is to have a `client role` for each possible combination of entity-operation. In our example, this would result in 12 possible roles: `user_create`, `user_read`, `user_update`, `user_delete` etc.

After that, you can create composite realm roles like `app_user` that will contain all the roles that represent the operations the the role holder can do on resources.

### Composite Roles (Reversed) Approach
The same idea as the **Composite Roles Approach**, but use `realm roles` instead for the authorities.

The advantage in doing something like this would be that you can create one single realm per application, make the realm roles for it once (the ones that look like `user_create` and `employee_delete`) and then the client will have the composite roles that will be assigned to users.

This is useful because you can create a *single* realm on the Keycloak instance and then reuse that realm for all the instances, only creating a new client for each of the instances.

The **disadvantage** would be the fact that the users of all instances would be part of a single realm, which means that there can't be multiple users across different instances that use the same name.