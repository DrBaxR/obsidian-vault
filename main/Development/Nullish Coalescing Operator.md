Tags: #js #frontend 
Created: 2022-08-20 15:08
References: 

# Nullish Coalescing Operator (??)
In [[JavaScript]] it is the logical operator that returns the right hand side of the operation when the left hand side is **null** or **undefined**.

```js
const foo = null ?? 'default string';
console.log(foo);
// expected output: "default string"

const baz = 0 ?? 42;
console.log(baz);
// expected output: 0
```

The logical or (||) operator was used for this purpose before this operator was introduced, however it used to have a pitfall: returning the right hand for any *falsy* value.

```js
let count = 0;
let text = "";

let qty = count || 42;
let message = text || "hi!";
  
console.log(qty); // 42 and not 0
console.log(message); // "hi!" and not ""
```

## Resources
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator