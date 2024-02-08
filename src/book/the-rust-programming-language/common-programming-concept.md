# Common Programming Concept

## Convention

- snake_case for function and variable
- use ***//*** for comment

---

## Shadowing

- ability to declare variable with the same name as a previous defined variable
- first variable **_is shadowed_** by the second
- it is different from mut, as type can be changed and after shadowing the previous variable is gone

```rust
fn main() {
    let x = 5;

    let x = true;
    
    // creates a scope
    {
        let x = false;
        println!("The value of x in the inner scope is: {x}");
    }

    println!("The value of x is: {x}");
}
```

---

# Data Types

- rust is statically typed -> all types info is confirmed in compile time

## Primitives

### Constant

- use UPPER_CASE for constant
- **not** allow to use mut
- type of the value must be annotated
- can be used in **_any scope_** including global scope, while let can only be used in function

```rust
const RETENTION_DAY: u32 = 30;
```

### Scalar

- Integer, u8, i8, u32, i32..., isize, usize (depends on the architecture)
- Floating-Point
    - all floating point types are signed, f32, f64
    - IEEE-754 standard
- Boolean
    - one byte
- Character
    - 4 bytes
    - unicode Scalar Value
    - zero-width spaces is valid

### Overflow

> It will panic will overflow occurs in debug mode, two's complement wrapping instead of panic in release mode

#### Explicitly handle overflow

- Wrap in all modes with the wrapping_* methods, such as wrapping_add.
- Return the None value if there is overflow with the checked_* methods.
- Return the value and a boolean indicating whether there was overflow with the overflowing_* methods.
- Saturate at the value’s minimum or maximum values with the saturating_* methods.

### Unit

- special **_type_** represent empty tuple
- expressions implicitly return the unit value if they don’t return any other value

### Tuple

- fixed length, once declared, cannot grow or shrink
- multi-types values is allowed

```rust
fn main() {
    // init
    let tup: (i32, f64, u8) = (500, 6.4, 1);
    let tup = (500, 6.4, 1);

    // access
    let five_hundred = tup.0;
    let six_point_four = tup.1;
    let one = tup.2;
    
    // no field `3` on type occurs when accessing invlid index
    print!("{}", tup.3);

    // destructure by pattern matching
    let (x, y, z) = tup;
}
```

### Array

- fixed length, same type
- allocated on stack
- index out of bounds error occurs when accessing invalid index

```rust
fn main() {
    let a = [1, 2, 3, 4, 5];

        // [type, num of elements]
    let a: [i32; 5] = [1, 2, 3, 4, 5];

    let a = [3; 5]; //same as let a = [3, 3, 3, 3, 3]

    // access
    let first = a[0];
    let second = a[1];
}
```

***

## Function

> made up of a series of **_statement_** and optionally end with an **_expression_**

- can be defined anywhere as long as it can be seen by the caller
- argument type of each parameter must be declared
- return type must be declared explicitly
- value can be return through the return keyword or return the **last expression** implicitly
- ***unit*** is returned implicitly for function that don't have return value

### Statement

> instructions that perform some action and ***do not return*** a value

```rust
fn main() {
    // error, statement don't return valures
    let x = (let y = 6);
}
```

- *Function* ***definitions*** are also statements -> the entire function is statement -> cannot assign fn to variable

### Expression

- evaluate to a value
- Expressions **_do not include_** ending semicolons.
- ***Calling*** *a function* is an expression
- A new scope block created with ***curly brackets*** is an expression

```rust
fn main() {
    let y = {
        let x = 3;       // <- this {} block is an expression
        x + 1            // this line don't hv ; which tells it is an expression
    };

    println!("The value of y is: {y}");
}
```

## Flow Control

- non-boolean type is not allowed in condition expression

### if

- if is a statement -> can assign it to a variable

### Iteration

- three kinds of iteration: ***loop***, ***while***, ***for***

### loop

- run forever, until explicitly exit by **_break_**
- **_continue_** is allowed in loop
- label is allowed

### return values from break

```rust

fn main() {
    let mut counter = 0;

    let result = loop {
        counter += 1;
        if counter == 10 {
            break counter * 2;
        }
    };

    println!("The result is {result}");
}
```

### while

- like common while loop in other language

### for loop

using break to return value is not allowed

```rust
fn main() {
    for number in (1..4).rev() {
        println!("{number}!");
    }
    

    let a = [10, 20, 30, 40, 50];

    for element in a {
        println!("the value is: {element}");
    }
}
```