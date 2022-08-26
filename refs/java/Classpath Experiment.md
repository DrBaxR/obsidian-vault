Tags: #reference 
Created: 2022-08-26 21:08

# Classpath Experiment
This will illustrate how the [[Java]] [[Classpath]] works via an example.

## Setup
The first thing neeed is to create a `Main.java` file containing:
```java
public class Main {  
    public static void main(String[] args) {  
        System.out.println("test");  
    }  
}
```

Compile and test if you can run this by using the following commands:
```sh
javac ./Main.java
java Main
```

## Creating the library
Create a new folder `adding` with a new file `Adder.java` in it which contains the following code
```java
package adding;  
  
public class Adder {  
    public int add(int a, int b) {  
        return a + b;  
    }  
}
```

Compile the `Adder` class and package it in a [[JAR]] by using the following commands
```sh
javac adding/Adder.java
jar cf adding-lib.jar adding/Adder.class
```
This will produce a [[JAR]] file which is our 'library'. Move this file in a new directory called `lib` and delete the `adding` folder.

## Using the library
Modify the `Main.java` file like this so it uses the library we just created
```java
import adding.Adder;  
  
public class Main {  
    public static void main(String[] args) {  
        Adder adder = new Adder();  
        System.out.println(adder.add(1, 2));  
    }  
}
```

If you try to compile the file as we did before it will lead to compilation errors, since the compiler can't find the `Adder` class.

To fix this, we need to include the library we created in the [[Classpath]] when running the compile command by using the `-cp` parameter
```sh
javac -cp "lib/adding-lib.jar" ./Main.java
```

This will produce a `Main.class` compiled file which won't be runnable (*who would have thought*) without adding the library to the classpath. This can be done like this
```sh
java -cp "lib/adding-lib.jar:." Main
```

**Note:** When running the program you also need to add the current directory to the classpath so the [[JVM]] knows where it can find the `Main` class.

## Resources
my brain