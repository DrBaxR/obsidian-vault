---
tags: 
 - backend
 - docs
 - java
---

# OpenAPI Annotations

Here are some useful annotations that you can use to document endpoints in [[Java Enterprise]] using the [[OpenAPI]] standard:
- `@Operation`: Describe the operation that you do at the endpoint
```java
@Operation(
	summary = "Set the greeting prefix", 
	description = "Permits the client to set the prefix part of the greeting (\"Hello\")"
)
```
- `@RequestBody`: Describe the body of the request
- `@Content`: Describe content of something (for example of a body/response)
- `@Schema`: Describe schema of something (for example of a request's body)
- `@ExampleObject`: Describe an example object for the request
```java
@RequestBody( 
	name = "greeting", 
	description = "Conveys the new greeting prefix to use in building greetings", 
	content = @Content( 
		mediaType = "application/json", 
		schema = @Schema(implementation = GreetingMessage.class), 
		examples = @ExampleObject( 
			name = "greeting", 
			summary = "Example greeting message to update", 
			value = "New greeting message"
		)
	)
)
```
- `@Consumes`: Describes the media type that the endpoint consumes
- `@Produces`: Describes the media type that the endpoint produces

For more details on the annotations used, check [this](https://download.eclipse.org/microprofile/microprofile-open-api-3.0/microprofile-openapi-spec-3.0.html#_detailed_usage_of_key_annotations) out.