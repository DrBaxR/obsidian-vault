Tags: #reference 
Created: 2022-08-26 22:08

# What is the JVM
The [[JVM]] is a program whose purpose is to execute other programs. It has 2 primary functions:
- allow [[Java]] programs to run on any machine (*write once, run anywhere*)
- manage and optimize memory

**Technically:** *The JVM is the specification for a software program that executes code and provides the runtime environment for that code.*
**Everyday:** *The JVM is how we run our Java programs. We configure the JVM's settings and then rely on it to manage program resources during execution.*

The JVM has three aspects: specification, implementation and instance.

## Specification
First the JVM is a specification, which means that its *implementation details* are not described in the spec:

*"To implement the Java virtual machine correctly, you need only be able to read the `class` file format and correctly perform the operations specified therein."* (all it has to do is run [[Java]] programs correctly)

## Implementation
Implementing the specification results in an actual software program which is a JVM implementation.

Typically, you download and install the JVM as a bundled part of a [[JRE]] (Java Runtime Environment).

## Instance
After the spec has been implemented and released as a software product, you may download and run it as a program. This download is called an instance of the JVM.

Most of the time when people are talking about the "JVM", they are referring to its instance.

## Loading and executing `class` files in the JVM
In order to run these files, the JVM depends on two things: the Java class loader and the Java execution engine.

### Class loader
In order to run a Java program, the JVM needs to first load the compiled `.class` files into a context (eg. a server) where they can be accessed. This is the *class loader*'s responsibility.

### Execution engine
Once the classes are loaded, the JVM begins to execute the code in each class. The execution engine handles this task. For all practical purposes, the execution engine actually **is** the *JVM instance*.

The JVM execution engine stands **between** the **program** (which demands file, network and memory *resources*) and the **operating system** (which provides those resources).


## Resources
https://www.infoworld.com/article/3272244/what-is-the-jvm-introducing-the-java-virtual-machine.html