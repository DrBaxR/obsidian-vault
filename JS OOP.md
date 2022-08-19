Tags: #js #frontend 

# JS OOP
## Overview
All the class stuff ([[OOP]]) from [[JavaScript]] is just syntax sugar. Under the hood it still uses prototypes.

In order to declare a class, you can use the `class` keyword and to make a constructor for that class, use the `constructor` keyword in that class.

```js
class Person {
	name;
	
	constructor(name) {
		this.name = name;
	}
	  
	introduceSelf() {
		console.log(`Hi! I'm ${this.name}`);
	}
}

// usage
const giles = new Person('Giles');
giles.introduceSelf(); // Hi! I'm Giles
```

In case the constructor is omitted, [[JavaScript]] creates one automatically that does nothing.

## Inheritance
For inheritance, you can use the `extends` keyword. When extending a class, in the constructor of the class you **must** first call the constructor of the parent class via the `super()` method.

```js
class Professor extends Person {
	teaches;

	constructor(name, teaches) {
		super(name);
		this.teaches = teaches;
	}
	
	introduceSelf() {
		console.log(`My name is ${this.name}, and I will be your ${this.teaches} professor.`);
	  
	grade(paper) {
		const grade = Math.floor(Math.random() * (5 - 1) + 1);
		console.log(grade);
	}
}

// usage
const walsh = new Professor('Walsh', 'Psychology');
walsh.introduceSelf(); // 'My name is Walsh, and I will be your Psychology professor'
walsh.grade('my paper'); // some random grade
```

In the subclass you can add extra functionality and overrride already existing things.

## Encapsulation
In order to make fields and methods private, you need to prefix them with `#`.

```js
class Example {
	publicField = 'my public field';
	#privateField = 'my private field';
	
	somePublicMethod() {
		this.#somePrivateMethod();
	}
	
	#somePrivateMethod() {
		console.log('You called me?');
	}
}

const myExample = new Example();
myExample.somePublicMethod(); // 'You called me?'
myExample.#somePrivateMethod(); // SyntaxError
```

## Resources
[https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Classes_in_JavaScript](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Classes_in_JavaScript)