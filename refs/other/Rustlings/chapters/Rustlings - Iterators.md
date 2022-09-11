Tags: #reference 
Created: 2022-09-11 15:09

# Rustlings - Iterators
[[Iterator]]s are essential when performing operations on elements within a collection. To get access to the iterator of a vector, you can do this:

```rust
let v1 = vec![1, 2, 3];

let v1_iter = v1.iter();

for val in v1_iter {
	println!("Got: {}", val);
}
```

As you can see, you can use a `for in` loop to go through the elements in an iterator.

All iterators implement a trait that is named `Iterator`, which only has a method called `next` that returns an `Option`. Here is a usage example of that function:

```rust
#[test]
fn iterator_demonstration() {
	let v1 = vec![1, 2, 3];

	let mut v1_iter = v1.iter();

	assert_eq!(v1_iter.next(), Some(&1));
	assert_eq!(v1_iter.next(), Some(&2));
	assert_eq!(v1_iter.next(), Some(&3));
	assert_eq!(v1_iter.next(), None);
}
```

### Consuming adaptors
The methods that call `next` are called *consuming adaptors*, because calling them uses the iterator. An example of this is the `sum` method

```rust
#[test]
fn iterator_sum() {
	let v1 = vec![1, 2, 3];

	let v1_iter = v1.iter();

	let total: i32 = v1_iter.sum();

	assert_eq!(total, 6);
}
```

**Note:** We can't use `v1_iter` after the call to `sum`, because the method takes ownership of the iterator we call it on.

### Iterator adaptors
They are methods that are defined on the `Iterator` trait that don't consume the iterator, instead they produce different iterators by changing some aspects of the original iterator.

```rust
let v1: Vec<i32> = vec![1, 2, 3];

v1.iter().map(|x| x + 1);
```

**Note:** This produces a warning, since iterators are lazy: they need to be consumed in order to be modified. Fix:

```rust
let v1: Vec<i32> = vec![1, 2, 3];

let v2: Vec<_> = v1.iter().map(|x| x + 1).collect();

assert_eq!(v2, vec![2, 3, 4]);
```

## Resources
[[Rustlings]]