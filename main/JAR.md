Tags: #java 
Created: 2022-08-25 19:08
References: [[Guide to Creating an Running a Jar File in Java]]

# JAR
A JAR (Java ARchive) is basically an archive that bunles up many [[Java]] class files together. This file can be created by using the `jar` command or by using a build tool, such as [[Maven]].

Besides the compiles `.class` files, the JAR also contains a `META-INF/MANIFEST.MF` file which contains meta information about the JAR (like the version number, entry point and such things).

Here is an example of how a `jar` usage would look like. The command below creates the jar containing the `Main.class` file and specifies that `Main` is the entry point of the application:
```sh
jar cfe jar-sample.jar Main src/Main.class
```