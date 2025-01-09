Tags: #abstract 
Created: 2024-11-02 18:11
References:
- https://marianogappa.github.io/software/2019/06/05/lets-build-a-sql-parser-in-go/
- https://en.wikipedia.org/wiki/Lexical_analysis#Lexical_grammar
- https://en.wikipedia.org/wiki/Abstract_syntax_tree

# Parsing
This document describes how parsing works (i.e. for parsing a language).

## Description
Generally speaking, the purpose of a parser is to take a chunk of text as input and spit out some meaningful representation of the text that is much simpler to use by the clients of the parser than the raw text.

Basically, it's something like "**casting**" a string to a "**type**".

Generally speaking a parser has 2 parts:
1. The lexical analysis (also called the **tokeniser**)
2. The syntactic analysis (which creates an [[AST]])

## Lexical Analysis
This part is responsible for splitting the input string into more strings (called tokens), each of them fitting in one of many categories, which vary based on the language that is being analyzed.

For example, this C expression:
```c
x = a + b * 2;
```

Could be split into these tokens:
```
[(identifier, x), (operator, =), (identifier, a), (operator, +), (identifier, b), (operator, *), (literal, 2), (separator, ;)]
```


## Syntactic Analysis
This part is responsible of taking in the tokens produced in the lexical analysis and interpreting them in order to produce an AST, which is the output of the parser.

**Note:** One approach to do this is to use a [[Finite State Machine]], which generally simplify the implementation of this part.