Tags: #reference 
Created: 2022-08-26 21:08

# What is a Classpath
While programming in [[Java]], you can use imports to split your code in multiple files. This is nice, however the [[JVM]] needs to know where to find the classes that you are importing. It would be impractical to have the [[JVM]] look through all the folders on your machine, therefore you need to specify where it should look for classes via the [[Classpath]].

Classpaths contain multiple entries which can be either:
- [[JAR]] files
- Paths to the top of package hierarchies

There are 2 ways you can set the [[Classpath]]:
- with environment variables
```sh
export CLASSPATH=/home/myaccount/myproject/lib/CoolFramework.jar:/home/myaccount/myproject/output/
```
- using the `-cp` parameter when starting java
```sh
java -cp "/home/myaccount/myproject/lib/CoolFramework.jar:/home/myaccount/myproject/output/"  MyMainClass
```

**Note:** The classpath value is a list of paths split by `:` (UNIX based systems) and `;` (Windows).

Since using environment variables has the same downsides as using global variables, it is preferrable to use the `-cp` way of specifying it.

Example: [[Classpath Experiment]]

## Resources
https://stackoverflow.com/questions/2396493/what-is-a-classpath-and-how-do-i-set-it