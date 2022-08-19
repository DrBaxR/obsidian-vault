---
tags:
 - backend
 - java
---

OpenAPI was created to be a standard of how [[API]]s are documented. It is a specification that is implemented by many providers, such as [[Helidon]].

This note will describe OpenAPI from the perspective of [[Helidon MP]]. The first thing you need to do is add some dependencies to `pom.xml`:

```xml
<dependencies>  
    <dependency>        
	    <groupId>org.eclipse.microprofile.openapi</groupId>  
        <artifactId>microprofile-openapi-api</artifactId>  
    </dependency>    
    <dependency>        
	    <groupId>io.helidon.microprofile.openapi</groupId>  
        <artifactId>helidon-microprofile-openapi</artifactId>  
        <scope>runtime</scope>  
    </dependency>
</dependencies>
```

After dependencies were added, all you need to do is add [[Annotation]]s to the methods that handle a request.

Some of the annotations that you can use:
- `@Operation` gives information about endpoint
- `@APIResponse` describes the [[HTTP]] response and declares media types and contents

```java
@GET  
@Operation(  
    summary = "A simple test endpoint",  
    description = "Informs the used that this is a test endpoint"  
)  
@APIResponse(  
    description = "Just plain text containing the string of text",  
    content = @Content(  
        mediaType = MediaType.TEXT_PLAIN,  
        schema = @Schema(implementation = String.class)  
    )  
)  
@Produces(MediaType.TEXT_PLAIN)  
public String test() {  
    return "this is a test";  
}
```

Besides using [[Annotation]]s, you can also use a static file and a tool like [[Swagger]] which can generate an OpenAPI document file based on the static file.

In order to access the generated document, you have to do a GET request at the `/openapi` endpoint. The document that gets generated is a [[YAML]] file. Here's an example of the document that gets generated for the endpoint above:

```yaml
info: 
  title: Generated API
  version: '1.0'
openapi: 3.0.3
paths:
  /test: 
    get: 
      description: Informs the used that this is a test endpoint
      responses:
        default: 
          content:
            text/plain: 
              schema: 
                type: string
          description: Just plain text containing the string of text
      summary: A simple test endpoint
```

### Resources
https://helidon.io/docs/v3/#/mp/openapi