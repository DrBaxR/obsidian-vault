---
tags:
 - backend
 - java
---

It is an Eclipse [[MicroProfile]] 5.0 runtime that allows the [[Java Enterprise]] community to run microservices in a portable way. It is build on top of [[Helidon SE]], providing a much nicer, declarative interface ([[Java Enterprise]] style).

## Starting
To create a new Helidon project, you can start from the following [[Maven]] archetype (you need at least [[Java]] 17 to be able to develop Helidon projects):

**Note:** You may need to change the version of the archetype _or_ to put the values of the keys in quatation marks (**"**).
```sh
mvn -U archetype:generate -DinteractiveMode=false \ 
	-DarchetypeGroupId=io.helidon.archetypes \
	-DarchetypeArtifactId=helidon-quickstart-mp \
	-DarchetypeVersion=3.0.0 \
	-DgroupId=io.helidon.examples \
	-DartifactId=helidon-quickstart-mp \
	-Dpackage=io.helidon.examples.quickstart.mp
```

In case you want to create a MP project that can be run from dirrectly from the IDE, without using [[Maven]], you can create a Main class that contains the following:
```java
package io.helidon.examples.quickstart.mp;  
  
import io.helidon.config.Config;  
import io.helidon.microprofile.server.Server;  
  
public class Main {  
    public static void main(String[] args) {  
        Config config = Config.builder().build();  
  
        Server server = Server.builder()  
            .config(config)  
            .build();  
  
        server.start();  
    }  
}
```
**Note:** This may cause problems down the way, so it is better to run the project by using [[Maven]].

### Resources
https://helidon.io/docs/v3/#/mp/introduction
https://helidon.io/docs/v3/#/mp/guides/quickstart
