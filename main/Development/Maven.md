---
tags:
 - java
 - devops
 - maven
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
There are many other [[Maven Goal]]s that can be executed. For example, you can generate a _website_ for your project by running `mvn site`. Or, if you want to delete the `target` folder, you can run `mvn clean`.

## Versions
The `-SNAPSHOT` suffix of a version refers to the fact that that is the latest version of the code, which means that there is no guarantee that the code is _stable_ or _unchanging_. In other words, the **snapshot** version of a project is the version before the **release** version.

## Plugins
Customising the build of a Maven project is done by adding or reconfiguring plugins. This can be done in the `pom.xml` file, where plugins can be added just like dependencies: they will be automatically downloaded and used with the specified configuration.

In order to configure a plugin, you need to specify the properties that can be configured and their values in the `configuration` element. It's possible to add [[Maven Goal]]s or configure them. More information about this can be found by reading about the [[Maven Build Lifecycle]].

## Resources
https://maven.apache.org/guides/getting-started/