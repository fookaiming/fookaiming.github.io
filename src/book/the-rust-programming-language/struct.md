# Struct

## Operation

```rust
// define
#[derive(Debug)]
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}

fn main() {
    // new
    let user1 = User {
        active: true,
        username: String::from("someusername123"),
        email: String::from("someone@example.com"),
        sign_in_count: 1,
    };

    // new from another instance
    let user2 = User {
        sign_in_count: 100,
        ..user1             // <- must state in last line 
    };

    // note user1 is invalid here,
    // since the ownership of username and email is transferred to user2

    print!("{:#?}", user2)
}
```

## Tuple Struct

- struct with fields that don't have a name

```rust
struct Color(i32, i32, i32);

struct Point(i32, i32, i32);

fn main() {
    let black = Color(0, 0, 0);
    let origin = Point(0, 0, 0);
}
```

## Unit-Like Struct

- struct with no field
- use for implementing trait

```rust
struct AlwaysEqual;

fn main() {
    let subject = AlwaysEqual;
}
```

## Method

- method calling support ***automatic referencing and dereferencing*** -> implcitly induce from method receivers
- defined within the context of struct, enum or trait
- the first parameter is always ***self***

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    // &self is shorthand of self: &Self
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    // automatic referencing and de-referencing here: rect1.area() is same as (&rect1).area()
    println!("The area of the rectangle is {} square pixels.", rect1.area()
    );
}
```

## Associated Functions

- All functions defined within an impl block are called associated functions, method is a subset of Associated Functions
- associated function can be defined ***without*** *self* parameter, like static method in java
- use `Struct_Name::FunctionName` to invoke the function

```rust
impl Rectangle {
    fn square(size: u32) -> Self {
        // Self -> alias of Rectangle
        Self {
            width: size,
            height: size,
        }
    }
}

fn main() {
    Rectangle::square(2)
}
```