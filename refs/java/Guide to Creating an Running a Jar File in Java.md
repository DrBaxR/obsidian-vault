Tags: #reference 
Created: 2022-08-25 18:08

# Guide to Creating an Running a Jar File in Java
A [[JAR]] is basically an archive that bunles up many [[Java]] class files together.

Before creating a [[JAR]] file, we need a class with a `main` method which will be the entry point of the application:
```java
public class Main {  
    public static void main(String[] args) {  
        System.out.println("Hello World!");  
    }  
}
```

The first step to creating a JAR file is to compile all the source files. This command can be used to compile all the `.java` files in the `./src` directory:
```sh
javac src/*.java
```

In case `Main.java` is the only file in the directory, the command above will only produce a `Main.class`
file in the `src` directory.

After having the compiles `.class` files, we can use the `jar` command with the flags `c` for creating a file and `f` for specifying the name of the file:
```sh
jar cf jar-sample.jar src/Main.class
```

[[JAR]]s also contains a `META-INF` directory that contains a `MANIFEST.MF` file. This file contains meta information about files within jar files, such as the entry point of the application.

By using the `e` option, the manifest file will be automatically added to the jar and contain the *entry point*.
```sh
jar cfe jar-sample.jar Main src/*.class
```

This is how the manifest file produced by the command above looks like:
```
Main-Class: com.baeldung.jar.JarExample
```

**Note:** Using the `jar` command will lead to creating the JAR with a structure that reflects the hierarchy of files from the place where you ran the command. For example, if you run the command from above, the JAR will be produced with the following structure:
```
jar-sample.jar
	src
		Main.class
	META-INF
		MANIFEST.MF
```
This will lead to a `ClassNotFoundException` when trying to run the JAR with `java -jar jar-sample.jar`, since the path to the entry point is actually `src/Main` and not `Main` (note the fact that `Main.class` still is in the root package). The only ways of fixing this would be to properly run the command from the root package **OR** to manually reajust the structure of the JAR.

## Resources
https://www.baeldung.com/java-create-jar