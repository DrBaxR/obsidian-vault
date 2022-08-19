---
tags:
 - js
---

A [[JavaScript]] function that returns a generator object. They are functions that can be exited and re-entered later (variable bindings get saved across re-entrances).

You use _yield_ to return a certain value. The next run of the generator starts from previous yield.

*yield* is also a thing - it can be used to exhaust another generator (generator in generator bullshit) - check resources for more.

## Syntax
```javascript
function* generator(i) {
	yield i;
	yield i + 10;
}

const gen = generator(10);

console.log(gen.next().value); // expected output: 10
console.log(gen.next().value); // expected output: 20
```

## Example
```javascript
function* idMaker() {
	var index = 0;
	while (true)
		yield index++;
}

var gen = idMaker();

console.log(gen.next().value); // 0
console.log(gen.next().value); // 1
console.log(gen.next().value); // 2
console.log(gen.next().value); // 3

// ...
```

### Resources
[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function)