Tags: #reference 
Created: 2022-09-18 13:09

# Intro to Reactive Programming
[[Reactive Programming]] is a paradigm that is centered around the idea of data streams that you can combine and transform in order to handle asynchronously.

Reactive programming consists of the set of principles and there are certain specifications that say how it should be implemented (eg. `reactivex.io`, who implemented [[RxJS]]).

The stream that is emitting the events (data) is called the **subject**. The stream that comsumes the events is called the **observer**.

In order to manipulate the data, you manupulate the stream of data by applying certain functions to it.

## Imperative vs. Functional (Declarative)
1. Imperative:
	- describes how the program operates
	- uses statements to change the **state of the program**
2. Functional:
	- descrobes *what* the program should accomplish
	- ideally **stateless**
	- evaluate functions and create expressions instead of statements

## Resources
https://www.youtube.com/watch?v=KOjC3RhwKU4