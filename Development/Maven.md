---
tags:
 - java
 - devops
---
# Maven
Maven is an attempt _to apply patterns to a project's build infrastructure in order to promote comprehension and productivity by providing a clear path in the use of best practices_. It is mainly used in the [[Java]] ecosystem.

The quickest way to create a Maven project is by using a [[Maven Archetype]]. To create a simple maven project, use:

```sh
mvn -B archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4
```

This comamnd (among other things), creates a new folder (`my-app`) that contains a `pom.xml`
file, which contains the [[POM]] for this project. Something that is note-worthy is the fact that everything revolves around the notion of a **project**.

The structure of a Maven project (generally) follows a convention: *application* sources are found in `${baseDir}/src/main/java` and test *sources* reside in `${baseDir}/src/test/java`.

## Compile
In order to compile the source files, you can run `mvn compile`, which will spit out the `.class` files in the `${baseDir}/target/classes` folder. **Note:** Running any Maven command will first download all the dependencies and cache them. In case they are already cached, it will no longer download them.

Compiling test files is just as simple: run `mvn test`, which will generate the compiled files in the `${baseDir}/target/test-classes`.

## Generating the [[JAR]]
In order to create a JAR with Maven, you can run `mvn package`, which will spit the JAR out in the `${baseDir}/target` folder. To install the artifact that was generated in your local [[Maven Repository]], run `mvn install`.

## Other commands
There are many other [[Maven Goals]] that can be executed. For example, you can generate a _website_ for your project by running `mvn site`. Or, if you want to delete the `target` folder, you can run `mvn clean`.

## `SNAPSHOT` version



### Resources
https://maven.apache.org/guides/getting-started/