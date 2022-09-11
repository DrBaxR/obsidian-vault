Tags: #reference 
Created: 2022-09-11 15:09

# Rustlings - Traits
Traits in rust are pretty much like interfaces or abstract classes in *Java*. It defines functionality that a particular type can share to other types.

```rust
pub trait Summary {
    fn summarize(&self) -> String;
}

pub struct NewsArticle {
    pub headline: String,
    pub location: String,
    pub author: String,
    pub content: String,
}

impl Summary for NewsArticle {
    fn summarize(&self) -> String {
        format!("{}, by {} ({})", self.headline, self.author, self.location)
    }
}
```

It is also possible to provide default implementations for certain trait methods.

To pass a trait implementation as a function parameter, you can use the `impl` keyword in the type of the parameter. It is also possible to specify that the parameter implements multiple traits

```rust
// single
pub fn notify<T: Summary>(item: &T) {
    println!("Breaking news! {}", item.summarize());
}

// multiple
pub fn notify(item: &(impl Summary + Display)) {
```

In order to make trait bounds more readable, the `where` clause exists. Here is some code written without using `where` and some code written using it.

```rust
// no where
fn some_function<T: Display + Clone, U: Clone + Debug>(t: &T, u: &U) -> i32 {

// where
fn some_function<T, U>(t: &T, u: &U) -> i32
    where T: Display + Clone,
          U: Clone + Debug
{
```

When you use generics it is possible to only implement some methods for the types that implement a certain trait. These methods will only be available when the generic type implements the said traits.

```rust
use std::fmt::Display;

struct Pair<T> {
    x: T,
    y: T,
}

impl<T> Pair<T> {
    fn new(x: T, y: T) -> Self {
        Self { x, y }
    }
}

impl<T: Display + PartialOrd> Pair<T> {
    fn cmp_display(&self) {
        if self.x >= self.y {
            println!("The largest member is x = {}", self.x);
        } else {
            println!("The largest member is y = {}", self.y);
        }
    }
}
```

It is also possible to implement traits for types that implement another trait. This mechanism is called *blanket implementations*. A good example would be what the standard library does with the `ToString` trait.

```rust
impl<T: Display> ToString for T {
    // --snip--
}
```

## Resources
[[Rustlings]]