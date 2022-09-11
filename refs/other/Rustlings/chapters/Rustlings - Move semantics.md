Tags: #reference 
Created: 2022-09-11 15:09

# Rustlings - Move semantics
## Move semantics
### Ownership
Ownership is a set of rules that [[Rust]] uses to manage memory:
- Each value in rust has an owner
- There can only be one owner at a time
- When the owner goes out of scope, the value gets dropped

```rust
let s1 = String::from("hello");
let s2 = s1;  

println!("{}, world!", s1); // compile error
```

In the code above, rust does not actually copy the memory of the string in the second line. It only copies the *pointer* to the memory, which is a structure that has 3 properties: `ptr`, `len` and `capacity`.

Trying to call something using `s1` will cause a compilation error, because of the way rust works: when you assign some variable to another variable, rust does a *shallow copy* of the values **and** it invalidates the previous location. This is called a **move**.

In case you want a *deep copy* of the data, you can call the `clone` method, which will also copy the heap data.

```rust
let x = 5;
let y = x;

println!("x = {}, y = {}", x, y); // works just fine
```

The sequence above seems to contradict what we discussed, but there is a catch. The data that is on the stack (its size *is known at compile time*), then the value gets **copied**, not moved. This happens for the types that implement the `Copy` trait.

#### Ownership and functions
Passing a value into a function parameter will move/copy, just as assignment does. Returning a value can also transfer ownership

### References and borrowing
Since taking ownership in functions and returning it afterwards is tedious, rust also has a borrowing mechanism.

A reference is an address we can follow to access the data stored at that address. A reference is guaranteed to point to a valid value for the life of that reference.

```rust
fn main() {
    let s1 = String::from("hello");

    let len = calculate_length(&s1);

    println!("The length of '{}' is {}.", s1, len);
}

fn calculate_length(s: &String) -> usize {
    s.len()
}
```

The ampersands indicate references and they allow you to refer to some value without taking ownership of it. If you try to moidfy some value you borrowed, it does not work.

#### Mutable references
In order to be able to modify a value that you borrowed, you need a *mutable reference* of it.

```rust
fn main() {
    let mut s = String::from("hello");

    change(&mut s);
}

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```

Mutable references have a big constraint to them: **if you have a mutable reference to a value, you can have no other references to that value**. Trying to do so will result in a compile error.

## Resources
[[Rustlings]]