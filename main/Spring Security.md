Tags: #backend #security 
Created: 2022-10-30 13:10
References: https://www.youtube.com/watch?v=her_7pa0vrg

# Spring Security
This note describes the things you can do with [[Spring]] security.

## Dependencies
With gradle, you can get the dependencies by adding this in your `build.gradle` file, at the `depednencies`:

```kotlin
implementation("org.springframework.boot:spring-boot-starter-security:2.7.5")
```

## Default behavior
If you have some endpoints when adding sprong boot security, they woll be automatically secured. If type them in the bowser (for `GET` requests only), you will see a form, which will make you type a user and a password (`user` and the password is in the terminal after starting your application).

This is called [[Form Based Authentication]] and will be discussed later.

## Basic auth
In order to add [[Basic Auth]] to your endpoints, you need to create a new class and annotate it with `@Configuration` and `@EnableWebSecurity`.

After doing that, you need do add the following method in the class:

```java
@Bean  
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {  
    http  
            .authorizeHttpRequests()  
            .anyRequest()  
            .authenticated()  
            .and()  
            .httpBasic();  
  
    return http.build();  
}
```

## Ant matchers
In case you want to whilelist some URLs from being authenticatted, you can use `.antMatchers()` and `.permitAll()`.

```java
@Bean  
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {  
    http  
            .authorizeHttpRequests()  
            .antMatchers("/index.html")  // +
			.permitAll()                 // +
            .anyRequest()  
            .authenticated()  
            .and()  
            .httpBasic();  
  
    return http.build();  
}
```

## User details service
You can configure how spring stores the users of the application, since the default behaviour (generating a single user with a random password) is not very useful.

The configure this, you need to provide a method that looks like this (returns a `UserDetailsService` and is annotated wit `@Bean`):

```java
@Bean  
public UserDetailsService userDetailsService() {  
    UserDetails root = User.builder()  
            .username("root")  
            .password("password")  
            .roles("DEFAULT")  
            .build();  
  
    return new InMemoryUserDetailsManager(root);  
}
```

**Note** that this will not be enough to make the user you set be usable in the authentication.

## Password encoder
The passwords that you save manually need to be encoded (the ones that you set in `InMemoryUserDetailsManager`). Spring does this with the help of a `PasswordEncoder`.

A password encoder needs to be provided to spring. There are multiple implementations, but the most popular one is `BCryptPasswordManager`. To provide the password encoder you need a config class that hass a method that returns it.

```java
@Configuration  
public class PasswordConfig {  
  
    @Bean  
    public PasswordEncoder passwordEncoder() {  
        return new BCryptPasswordEncoder(10);  // takes in the strength of the encoder
    }  
}
```

After providing it, you need to inject wherever you manually set a password and use it like this (context is the same as the example above):

```java
// ...
.password(passwordEncoder.encode("password"))
// ...
```
