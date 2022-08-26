Tags: #java 
Created: 2022-08-26 22:08
References: [[What is a Classpath]] [[Classpath Experiment]]

# Classpath
The [[Java]] classpath is basically a list of paths that tell the [[JVM]] where it should look to find the classes the application uses, since searching in all the machine's folders for classes is impractical.

Entries in the classpath can be either one of these:
- paths that lead to [[JAR]] files
- paths to the top of package hierarchies

The most convenient way to set the classpath is by using the `-cp` parameter:
```sh
java -cp "lib/adding-lib.jar:." Main
```