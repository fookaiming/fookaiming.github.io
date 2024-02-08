# Fight With Borrow Checker

## Case Study

### Returning a Reference to the Stack

```rust
fn return_a_string() -> &String {
    let s = String::from("Hello world");
    &s
    // s is dropped
}
```

#### Solution

- return String instead of reference

```rust
fn return_a_string() -> String {
    let s = String::from("Hello world");
    s
}
```

- string literal (live forever)

```rust
// 'static -> live forever
fn return_a_string() -> &'static str {
    "Hello world"
}
```

- defer borrow check with garbage collection

```rust
// At runtime, 
// the Rc checks when the last Rc pointing to data has been dropped
// then deallocates the data.
use std::rc::Rc;

fn return_a_string() -> Rc<String> {
    let s = Rc::new(String::from("Hello world"));
    Rc::clone(&s)
}
```

- shift the responsibility to the caller

```rust
fn return_a_string(output: &mut String) {
    output.replace_range(.., "Hello world");
}
```

> 💡 choose the solution base on your need, try to think about How long should my string live? Who should be in charge of
> deallocating it?