---
tags:
 - js
---

[[JavaScript]] objects are pretty weird, so this note will describe how they work.

## Object literals
To create a function on an object (using an **object literal**) you can either use the syntax from properties or you could use a simpler syntax as you can see in the snippet below:

```js
// normal syntax
const person = {
	// ...
	bio: function() {
		// ...
	}
};

// shorthand syntax
const person = {
	bio() {
		// ...
	}
}
```

You can also access properties of objects via the **bracket notation**. This is similar to the way you access elements in an array and it is basically the same thing, but instead of using numbers, you use strings (which makes JS objects maps).

```js
person.age
person.name.first

person['age']
person['name']['first']
```

Properties that don't exist on an object can also be assigned to it. This combined with the fact that you can use the bracket notation to access properties make objects really flexible.

```js
const person = {}
const myDataName = 'height';
const myDataValue = '1.75m';

person[myDataName] = myDataValue;
```

**this** refers to the current object the code is being written in. For the first example of code, this keyword in the bio function would refer to the _person_ object.

## Constructors
Only using object literals sucks dick. You have to keep repeating yourself when creating objects (if they have the same methods). It would be nice to be able to describe the shape/blueprint of an object and after that create as many objects from that blueprint as we want. This can be done with a function:

```js
function createPerson(name) {
	const obj = {};
	obj.name = name;
	obj.introduceSelf = function () {
		console.log(`Hi! I'm ${this.name}.`);
	}
	return obj;
}

const salva = createPerson('Salva');
salva.name;
salva.introduceSelf();
```

The code above works fine, but it is pretty long and does a bunch of things that irrelevant to the blueprint (it creates an object and returns it for later use). This functionality will be done in every creation function which gets repetitive. Therefore, you can use the **new** keyword. **new** does these things:
1.  create a new object
2.  bind **this** to the new object, so you can reference the new object inside the constructor
3.  run the code in the constructor
4.  return the new object

```js
function Person(name) {
	this.name = name;
	this.introduceSelf = function () {
		console.log(`Hi! I'm ${this.name}.`);
	}
}

const salva = new Person('Salva');
salva.name;
salva.introduceSelf();
```

This is basically how a constructor function gets called.

## Prototypes
Prototypes are the mechanism by which JS objects inherit features from one to another. Every JS object has a prototype, which is also an object, so the prototype also has a prototype. This goes on until the prototype of an object is null. This forms the **prototype chain**, which allows **inheritance**.

When you access some property on an object and that property is not found on the actual object, JS looks in the prototype. This goes on until the property is found. In case it is not found, _undefined_ in returned. To get the prototype of an object, use _Object.getPrototypeOf(myObject)_.

```js
const myDate = new Date();
let object = myDate;
  
do {
	object = Object.getPrototypeOf(object);
	console.log(object);
} while (object);
  
// Date prototype
// Object prototype
// null
```

Back to the prototype chain. Since it takes the first property it finds, starting from the actual object, you can **shadow properties** (something like overriding them).

There are a few ways to create the prototype of an object, but here's two: use _Object.create()_ or use constructor.

```js
const personPrototype = {
	greet() {
		console.log('hello!');
	}
}

const carl = Object.create(personPrototype);
carl.greet(); // hello!
```

In JS, all functions have a property named _prototype_. When you call a function as a constructor, this property is set as the prototype of the newly constructed object. So if we set the _prototype_ of a constructor, we can ensure that all objects created with that constructor are given that prototype.

```js
const personPrototype = {
	greet() {
		console.log(`hello, my name is ${this.name}!`);
	}
}

function Person(name) {
	this.name = name;
}

Person.prototype = personPrototype;
Person.prototype.constructor = Person;
```

Last line is required because after setting _Person.prototype = personPrototype;_ the property points to the constructor for the _personPrototype_, which is Object rather than Person (because _personPrototype_ was constructed as an object literal).

## Own properties
Objects that get created with the constructor above have a _name_ property on the actual object and a _greet()_ method in the prototype. This is a common pattern, because methods on objects are generally the same for all objects, but we want each objects have their own values for data properties (think of things on the prototype as static members of a class or something).

Own properties are properties that are defined on the object. You can check if an object has an own property by using _Object.hasOwn()_.

```js
const irma = new Person('Irma');

console.log(Object.hasOwn(irma, 'name')); // true
console.log(Object.hasOwn(irma, 'greet')); // false
```

### Resources
[https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects)