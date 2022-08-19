---
tags:
 - docs
 - java
 - backend
---
# Swagger
From what I gathered, Swagger can be used for many things, such as API development, testing and documentation.

The main reson why I heard people use it is because of the UI it produces for a [[OpenAPI]] document. You can access [this](https://editor.swagger.io), which is the Swagger editor. Here, you can paste a document and it will automatically generate the UI for it.

All it is is a bunch of static files (imagine a simple `index.html` file) that does an HTTP request at the `/openapi` endpoint. After it gets the response, it just generates the UI from which you can also do requests at the endpoints that you see.
