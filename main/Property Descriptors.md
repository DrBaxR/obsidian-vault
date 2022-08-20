Tags: #js #frontend 
Created: 2022-08-20 15:08
References: 

# Property Descriptors
Property descriptors are the thing that describe a property (obviously) of a [[JavaScript]] object. There is one of those property descriptors for each property on the object. 

They contains information like: if the property should appear in the enumeration of properties, if the property can be written and stuff like that. They way you specify the descriptor of a property is with _Obejct.defineProperty()_. Example of providing custom getters and setters:

```js
const object1 = {
	propVal: 42
};

Object.defineProperty(object1, 'prop', {
	get() {
		console.log('getter called');
		return this.propVal;
	},
	set(value) {
		console.log('setter called with value ' + value);
		this.propVal = value;
	}
});
  
object1.prop = 12;
console.log(object1.prop);
// setter called with value 12
// getter called
// 12
```

## Resources
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty