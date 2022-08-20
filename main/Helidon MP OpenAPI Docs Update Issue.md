---
tags:
 - backend
 - java
---

# Helidon MP OpenAPI Docs Update Issue
The issue is that if you have a [[Helidon MP]] application and you are using the [[OpenAPI]] to generate your [[API]] documentation, you can generate the yaml document the first time (all good), however when you change something and call the `/openapi` endpoint, the document does not get updated.

This happens (at least the time I had this issue) because of [[Jandex]], because it caches annotation indices in the `/target` folder.

To fix this, you can run this [[Maven]] command: `mvn clean process-classes`.