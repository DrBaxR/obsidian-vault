Tags: #reference 
Created: 2022-09-11 15:09

# Rustlings - Vectors
Vectors are kinda like arrays, the difference being that they are stored on the heap, since their size is not known at compile time.

```rust
let v = vec![1, 2, 3];
```

To read elements from the vectors, you can use one of two ways

```rust
let v = vec![1, 2, 3, 4, 5];

let third: &i32 = &v[2]; // may cause index out of bounds panic
println!("The third element is {}", third);

let third: Option<&i32> = v.get(2);
match third {
	Some(third) => println!("The third element is {}", third),
	None => println!("There is no third element."),
}
```

A common error that is caused by the borrowing system

```rust
let mut v = vec![1, 2, 3, 4, 5];

let first = &v[0];

v.push(6);

println!("The first element is: {}", first);
```

To iterate over the elements in the array you can do something like this with a `for` loop

```rust
let v = vec![100, 32, 57];
for i in &v {
	println!("{}", i);
}
```

## Resources
[[Rustlings]]