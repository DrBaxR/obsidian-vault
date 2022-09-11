Tags: #reference 
Created: 2022-09-11 15:09

# Rustlings - Lifetimes
A **lifetime** is the scope is which a reference is valid. Most of the time lifetimes are impricit, just like most of the time types are implicit. Lifetimes are important for preventing *dangling references*, such as the case in the next code sequence.

```rust
fn main() {
    let r;

    {
        let x = 5;
        r = &x;
    }

    println!("r: {}", r);
}
```

Rust's *borrow checker* is what makes sure that all borrows are valid.

### Generic lifetimes in functions
This code does not compile, beacause rust needs to know what the lifetime of the returned reference is, which means that we will need to specify it:

```rust
fn main() {
    let string1 = String::from("abcd");
    let string2 = "xyz";

    let result = longest(string1.as_str(), string2);
    println!("The longest string is {}", result);
}

fn longest(x: &str, y: &str) -> &str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```

The lifetime annotations of references don't modify the lifetime in any way, the only thing they do is *describe the relationships of lifetimes of multiple references*.

```rust
&i32        // a reference
&'a i32     // a reference with an explicit lifetime
&'a mut i32 // a mutable reference with an explicit lifetime
```

Here is how lifetimes can be used in the function that did not compile before

```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```

*Ultimately, lifetime syntax is about connecting the lifetimes of various parameters and return values of functions. Once they’re connected, Rust has enough information to allow memory-safe operations and disallow operations that would create dangling pointers or otherwise violate memory safety.*

### Lifetimes in structs
So far all the structs had held owned types. It is possible to define structs to hold references, the only requirement being that we annotate the lifetime.

```rust
struct ImportantExcerpt<'a> {
    part: &'a str,
}

fn main() {
    let novel = String::from("Call me Ishmael. Some years ago...");
    let first_sentence = novel.split('.').next().expect("Could not find a '.'");
    let i = ImportantExcerpt {
        part: first_sentence,
    };
}
```

### Lifetime elision
There are some cases where lifetime can be inferred in functions (*lifetime elision*). There is a set of rules that are deterministic, which is what lifetime elision is:
1. Compiler assigns a lifetime parameter to each parameter that's a reference
2. If there is exactly one input parameter, that lifetime is assigned to all output lifetime parameters
3. If there are multiple input lifetime params by one of them is `&self` or `&mut self`, the lifetime of `self` is assigned to all output lifetime parameters

### Lifetime annotation in method definitions
For implementing methods on a struct with lifetimes, we use same syntax as that of generic type parameters.

```rust
struct ImportantExcerpt<'a> {
    part: &'a str,
}

impl<'a> ImportantExcerpt<'a> {
    fn level(&self) -> i32 {
        3
    }
}
```

**Note:** Lifetime not annotated in implementation of `level` because of rule 1.

### The static lifetime
It is a special lifetime that denotes thatthe affected reference can live for the entire duration of the program. An example is for string literals:

```rust
let s: &'static str = "I have a static lifetime.";
```

Most of the times when compiler suggesting the `'static` lifetime results from attempting to create a dangling reference, in which cases the solution is fixing that, NOT specifying the `'static` lifetime.

### Using generic types, trait bounds, and lifetimes together
```rust
use std::fmt::Display;

fn longest_with_an_announcement<'a, T>(
    x: &'a str,
    y: &'a str,
    ann: T,
) -> &'a str
where
    T: Display,
{
    println!("Announcement! {}", ann);
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```


## Resources
[[Rustlings]]