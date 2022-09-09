Tags: #reference 
Created: 2022-09-08 21:09

# Map of the Territory
The whole process is made up out of a bunch of phases.

![Inage with the process of compilation/iterpreting](http://www.craftinginterpreters.com/image/a-map-of-the-territory/mountain.png)

### Scanning
This is the first phase (also known as **lexing**). It takes a string (sequence of characters) as input and it spits out words or **tokens**. During this phase comments and whitespace (if it is irrelevant) is discarded.

### Parsing
Phase that takes tokens as input and spits out a **parse tree** (or **abstract syntax tree**). These trees are tree-like structures of which's nodes are the tokens. This tree respects a grammar.

A parser's job is also to report if there are any **syntax errors**.

### Static analysis
Up to this point everything is pretty much the same for all the panguages. In this phase language specifix things may come into play.

The first thing most languages do is figuring out where each **identifier** is defined (the **scope** of the variables). This is called **binding** or **resolution**.

If the language is statically typed, the types of the variables are also figured out and **type errors** are raised in case there are some.

All this semantic insight needs to be stored somewhere. There are a couple places where we can do that:
- store it as attributes in the syntax tree
- store it in a lookup table called the **symbol table**
- store it in a whole new data structure that reflects the semantics of the code more dirrectly

Everything up to this point is called the **front end**.

### Intermediate representations
The intermediate representation (**IR**) serves as an intermediary stage to reduce the effort needed to implement support for more languages.

Let's say you want to implement compilers for three languages and you want to target three platforms. This would mean that normally you'd have to write *nine* compilers. With IR, you only need to implement *one front end for each language* that produces the IR and *one back end for each target*.

### Optimization
This is the phase where since you know what the code means, you can swap it with code that has the same *semantics*, but is more performant.

### Code generation
This is where the **back end** starts. You take the semantics of the code and transform them to **code that the machine understands**.

There are two paths from here:
1. generate machine code, which ties the back end to the platform it targets but is more performant
2. generate code for a *virtual machine*, which is also called **bytecode** (because each instruction is usually a byte long), which is more portable but is less performant

### Virtual machines
If you produce byte code, you are not done (since no actual chip runs byte code), you also have two paths you can take:
1. you can translate the byte code to each platform you support (basically the byte code is used as an IR)
2. you write a **virtual machine**, a program that simulates a hipothetical chip. This is slower but you get more portability and simplicity

### Runtime
This is where the code is run. In the case of fully compiled languages, the runtime is inserted into the executable. In the case of languages that are interpreted or run in a VM, the runtime resides there.

# Shortcuts
### Single-pass compilers
Some compilers combine all the phases and have the parserdurrectly produce output code. These are called **single-pass compilers**.

### Transpilers
The compile your language's code to another language, letting that language's back end to do the heavy lifting.


## Resources
[[Crafting Interpreters]]