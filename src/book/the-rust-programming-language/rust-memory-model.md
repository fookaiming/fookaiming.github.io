`# Rust Memory Model

## What is memory manage?

> In most case, we mean manage heap, moreover, deallocate heap memory in right timing

## Memory(Heap) Manage Philosophy

- Garbage Collector
- explicit manage by programmer
- Ownership

---

## Manual Memory Management

> Manual Memory Management is not allow in Rust except using unsafe

## Design Principles of Memory Management in Rust

### Safety

- What is safety
    - never have undefined behavior in your programme, undefined behavior is especially dangerous for low-level programs
      with direct access to memory
        - About 70% of reported security vulnerabilities in low-level systems are caused by memory corruption

### Performance

- prevent undefined behavior at compile-time instead of run-time
    - zero cost abstraction -> check in compile time

---

## Memory Model of Rust

### Stack

> stack is managed by rust, **_stack frame_** is allocated when function is called and deallocated when return

- stack holds data associated with a specific function, while heap holds data that outlive a function
- when assigning types (with copy trait to variable) -> value is copied to that variable (on stack frame)
    - assign an integer variable to new variable
- access/copy on stack is faster than heap(loop up for new space for copy)
- stack is fixed size, cannot grow or shrink

### Heap

- All heap data must be owned by **_exactly one_** variable
- Heap data can only be accessed through its current owner, not a previous owner.

> If a variable **_owns_** a box, when Rust deallocates the variable's frame, then Rust deallocates the box's heap
> memory.

#### Box

- used to put data on the heap

#### Consider the following example

```rust
    fn main() {
    // copied 1_000_000 data on stak
    let a = [0; 1_000_000];
    let b = a;
    
    // copied 1 pointer on stack only
    let c = Box::new([0; 1_000_000]);
    let d = c;
}
```
