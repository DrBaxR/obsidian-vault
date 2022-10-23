Tags: #spring #backend 
Created: 2022-10-22 17:10
References: https://spring.io/guides/tutorials/rest/

# Spring REST Basics
This not will contain all the notable thigs that I learned while doing the [[Spring]] *Building REST services* tutorial.

The minimum a Spting application needs is a `public static void main` entry point that is annotated with `@SpringBootApplication`.

Some thigs used by the [[JPA]] are the `@Entity` annotation that lets you describe a data object. This annotation is used on the class and you can specify the id property by using the `@Id` and `@GeneratedValue` annotations on that property.

Besides useng the annotations that were described above, you also need to create a *repository* that you will use for persistance operations with that *entity*. For an entity called `Enployee`, this is how its repository would look like:

```java
interface EmployeeRepository extends JpaRepository<Employee, Long> {
}
```

## Annotations
In order to create mappers for certain paths and [[HTTP]] methods, you can use some of these annotations:
- `@Controller` - annotate the class that contains mappers (keep in mind that this you need another annotation together with this for REST)
- `@RestController` - this is the recommended annotation
- `@RestMapping(path="/demo")` - this annotation is applied to the controller class and path will be used to specify to which path the class corresponds
- `@GetMapping`, `@PostMapping`, `@DeleteMapping`, `@PutMapping` - these are used to map a method to a method to a path
- `@PathVariable` - you map a method parameter to a path variable
- `@RequestBody` - map method parameter to the requrst body

Another pretty cool thing that you can do is have a certain method describe what happens when an exception is thrown in a request handling method. Let's say you have this request handler:

```java
@GetMapping("/employees/{id}")  
Employee getOne(@PathVariable Long id) {  
    return repository.findById(id)  
            .orElseThrow(() -> new EmployeeNotFoundException(id));  
}
```

If you were to request an employee that does not exist, the default behavior would be to get a response with the status `500 Internal Error`, because an exception was thrown.

If you wanted to say: *whenever a handler throws `EmployeeNotFoundException`, send a response with the status `404 Not Found` and the exception message as the body*, you could do this:

```java
@ControllerAdvice  
public class EmployeeNotFoundAdvice {  
  
    @ResponseBody  
    @ExceptionHandler(EmployeeNotFoundException.class)  
    @ResponseStatus(HttpStatus.NOT_FOUND)  
    String employeeNotFoundHandler(EmployeeNotFoundException ex) {  
        return ex.getMessage();  
    }  
}
```
