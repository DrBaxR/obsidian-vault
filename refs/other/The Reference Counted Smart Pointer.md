Tags: #reference 
Created: 2022-09-11 19:09

# The [[Reference Counted]] [[Smart Pointer]]
These are used in case you need data that can have multiple owners.

*Imagine `Rc<T>` as a TV in a family room. When one person enters to watch TV, they turn it on. Others can come into the room and watch the TV. When the last person leaves the room, they turn off the TV because it’s no longer being used. If someone turns off the TV while others are still watching it, there would be uproar from the remaining TV watchers!*

```rust
enum List {
    Cons(i32, Rc<List>),
    Nil,
}

use crate::List::{Cons, Nil};
use std::rc::Rc;

fn main() {
    let a = Rc::new(Cons(5, Rc::new(Cons(10, Rc::new(Nil)))));
    let b = Cons(3, Rc::clone(&a));
    let c = Cons(4, Rc::clone(&a));
}
```

`Rc::clone` does not do a deep copy, it only increments the amount (count) of references.

## Resources
https://doc.rust-lang.org/book/ch15-04-rc.html