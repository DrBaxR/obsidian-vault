Tags: #reference 
Created: 2022-09-09 22:09

# Rustlings
## Intro
In [[Rust]], you can print stuff to the console with the `println!` command. You can also format the stuff you want to print using this command.

```rust
println!("Hello {}!", "World");
```

## Variables
By default, they are immutable and declared using the `let` keyword. You can also make them mutable by using `let mut`.

Constants can be declared using `const` and their type MUST be annotated.

### Shadowing
You can shadow the names of variables in an inner scope by declaring a new variable with the same name as a previous one

```rust
fn main() {
    let x = 5;

    let x = x + 1;

    {
        let x = x * 2;
        println!("The value of x in the inner scope is: {x}");
    }

    println!("The value of x is: {x}");
}

```

## Functions
Functions are defined with `fn`.

```rust
fn main() {
    print_labeled_measurement(5, 'h');
}

fn print_labeled_measurement(value: i32, unit_label: char) {
    println!("The measurement is: {value}{unit_label}");
}
```

### Statements and expressions
Statement are instructions that do some action and expressions evaluate to some resulting value.

```rust
fn main() {
    let y = {
        let x = 3;
        x + 1 // notice there is no semicolon
    }; // expression

    println!("The value of y is: {y}"); // statement
}
```

If you add a `;` at the end of a expression, you transform it into a statement.

### Return values
The last expression in a function is the return value or you can use `return`.

```rust
fn five() -> i32 {
    5
}

fn main() {
    let x = five();

    println!("The value of x is: {x}");
}

```

## `if` statements
You can use an `if` in a `let` statement since it is an expression.

```rust
fn main() {
    let condition = true;
    let number = if condition { 5 } else { 6 };

    println!("The value of number is: {number}");
}
```

## Data types
You have the classic types like `int`, `float`,... but they also include the amount of bits they have.

### Tuples
```rust
fn main() {
    let x: (i32, f64, u8) = (500, 6.4, 1);

    let five_hundred = x.0;

    let six_point_four = x.1;

    let one = x.2;
}
```

It is also possible to destructure tuples using this syntax

```rust
let cat = ("Furry McFurson", 3.5);
let (name, age) = cat;
```

### Arrays
```rust
fn main() {
    let a = [1, 2, 3, 4, 5];

    let first = a[0];
    let second = a[1];
}
```

You can initialize an array with the same element repeated a set amount of times with this syntax

```rust
let a = [3; 5];
```

## Slice Type
A slice lets you reference a contiguous sequence of elements in a collection rather than the whole collection.

### String slices
You can get a section of the string using string slices (which have a type of `&str`).

```rust
fn main() {
	let s = String::from("hello");
	
	let len = s.len();
	
	let slice = &s[3..len];
	let slice = &s[3..];
}
```

## Vectors
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

## Structs
This is how you define and instantiate structs

```rust
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}

fn main() {
    let user1 = User {
        email: String::from("someone@example.com"),
        username: String::from("someusername123"),
        active: true,
        sign_in_count: 1,
    };
}
```

You can also use `..` lust like the spread operator in [[JavaScript]] and you can also use field shorthand notation if field and value that you assign it to are the same.

### Method syntax
Methods are simitar to functions, the only difference being that they are defined in the context of a struct.

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!(
        "The area of the rectangle is {} square pixels.",
        rect1.area()
    );
}
```

You can also create *associated functions* (they are not methods, because they don't take `&self` as a parameter), functions that you can call like `String::from`.

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn square(size: u32) -> Self {
        Self {
            width: size,
            height: size,
        }
    }
}

fn main() {
    let sq = Rectangle::square(3);
}
```

It is also valid syntax to have multiple `impl` blocks.

## Enums
Rust enums can also encapsulate data, which makes them really powerful

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}
```

The enum above is equivalent to the following sequence if you use structs

```rust
struct QuitMessage; // unit struct
struct MoveMessage {
    x: i32,
    y: i32,
}
struct WriteMessage(String); // tuple struct
struct ChangeColorMessage(i32, i32, i32); // tuple struct
```

It is also possible to define methods on enums, also by using the `impl` keyword

```rust
impl Message {
	fn call(&self) {
		// method body would be defined here
	}
}

let m = Message::Write(String::from("hello"));
m.call();
```

### `Option` enum
Rust does not have `null`, but is has another way of expressing that a value may be missing: the Option enum

```rust
enum Option<T> {
    None,
    Some(T),
}
```

### Pattern syntax
You can use the `match` operator kind of like a switch

```rust
let x = 1;

match x {
	1 => println!("one"),
	2 => println!("two"),
	3 => println!("three"),
	_ => println!("anything"),
}
```

#### Destructuring
Destructuring can be used on structs

```rust
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let p = Point { x: 0, y: 7 };

    let Point { x, y } = p;
    assert_eq!(0, x);
    assert_eq!(7, y);
}
```

You can also use destructuring (pattern matching) in a `match` statement. This same mechanism can be used on enums

```rust
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let p = Point { x: 0, y: 7 };

    match p {
        Point { x, y: 0 } => println!("On the x axis at {}", x),
        Point { x: 0, y } => println!("On the y axis at {}", y),
        Point { x, y } => println!("On neither axis: ({}, {})", x, y),
    }
}
```

It's also possible to add extra conditions in match structures with *match guards*

```rust
let num = Some(4);

match num {
	Some(x) if x % 2 == 0 => println!("The number {} is even", x),
	Some(x) => println!("The number {} is odd", x),
	None => (),
}
```

#### `if let`
It is also possible to extract values from enums or other destructurable things by usign the `if let` feature of rust. The two sequences of code below do the same thing

```rust
let coin = Coin::Penny;
let mut count = 0;
match coin {
	Coin::Quarter(state) => println!("State quarter from {:?}!", state),
	_ => count += 1,
}
```

```rust
let coin = Coin::Penny;
let mut count = 0;
if let Coin::Quarter(state) = coin {
	println!("State quarter from {:?}!", state);
} else {
	count += 1;
}
```

## Strings
You can create a string with `String::new` or `String::from`. Concatenation can be done with `+` or with the `format!` macro.

A `String` is a wrapper over `Vec<u8>`. There are some problems in case your string does not have 8 byte encoed characters, more info in documentation if you ever need this.

There is also another string type `&str`, which is a slice of a string.

## Modules
- **Packages**: a [[Cargo]] feature that lets you build, test and share crates
- **Crates**: a tree of modules that produces a *library* or *executable*
- **Modules**: let you control organization
- **Paths**: a way of naming an item such as a struct, function or module

Only modules that are public (`pub`) can be accessed from outside. Without the `use` keyword, you would have to always write the full name of things, which can get repetitve. The `use` keyword brings the module name **ONLY** into the scope where `use` was used.

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

use crate::front_of_house::hosting;

pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
}
```

In order to split the modules in different files you need to specify the module name and also create a fiel with that path. An example for the code above would be:

src/lib.rs
```rust
mod front_of_house;

pub use crate::front_of_house::hosting;

pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
}
```

src/front_of_house.rs
```rust
pub mod hosting;
```

src/front_of_house/hosting.rs
```rust
pub fn add_to_waitlist() {}
```

## Options
The `Option` enum is the way rust deals will nullable values and many more:
-   Initial values
-   Return values for functions that are not defined over their entire input range (partial functions)
-   Return value for otherwise reporting simple errors, where None is returned on error
-   Optional struct fields
-   Struct fields that can be loaned or "taken"
-   Optional function arguments
-   Nullable pointers
-   Swapping things out of difficult situations

It is really common to use things like `if let` and `while let` with optionals.

## Error handling
For recoverable errors, the `Result` enum exists. This result can be used with `match` or you can call the `unwrap_or_else` method or other unwrap methods on it.

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

The `?` operator returns with the received error (if there is an error) *or* it unwraps the value from the result if it was `Ok`. This operator can be used in functions that return either `Result` or `Option`.

```rust
use std::fs::File;
use std::io;
use std::io::Read;

fn read_username_from_file() -> Result<String, io::Error> {
    let mut username_file = File::open("hello.txt")?;
    let mut username = String::new();
    username_file.read_to_string(&mut username)?;
    Ok(username)
}
```

## Generics
There are multiple places where you can use generics. Here they are:
- Function definitions
- Struct definitions
- Enum definitions
- Method definitions

```rust
struct Wrapper<T> {
    value: T,
}
  
impl<T> Wrapper<T> {
    pub fn new(value: T) -> Self {
        Wrapper { value }
    }
}
```

### Bounds
It is possible to specify what functionality a generic implements by using **bounds**.

```rust
// Define a function `printer` that takes a generic type `T` which
// must implement trait `Display`.
fn printer<T: Display>(t: T) {
    println!("{}", t);
}
```

## Traits
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

## Tests
Tests are rust functions that are annotated with the `#[test]` attribute. In these functions you can make assertions using things like `assert_eq!`.

```rust
#[cfg(test)]
mod tests {
    #[test]
    fn exploration() {
        assert_eq!(2 + 2, 4);
    }
}
```

In case the test should *panic*, then use the `#[should_panic]` attribute. If you don't want your tests to use assertions or to panic, you can make them return a `Result`.

```rust
#[cfg(test)]
mod tests {
    #[test]
    fn it_works() -> Result<(), String> {
        if 2 + 2 == 4 {
            Ok(())
        } else {
            Err(String::from("two plus two does not equal four"))
        }
    }
}
```

## Lifetimes
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
https://github.com/rust-lang/rustlings
