# Java Concurrency

## Common Problem when developing concurrent programme

1. starvation
    - prevent starvation by placing a timeout, waits for only a finite amount of time
2. deadlock
    - prevent deadlock by acquiring resources in a specific order
    - ***better avoid explicit locks and sharing mutable state***
3. ***race condition*** *(**should be eliminated at root**)*
    - two main factors lead to race condition
        1. JIT optimization
        2. Java Memory Model

## The root cause of race condition

### Shared mutability is pure evil

> When we have a nonfinal (mutable) field, each time a thread changes the value, we have to consider whether we have to
> put the change back to the memory or leave it in the registers/cache. Each time we read the field, we need to be
> concerned if we read the latest valid value or a stale value left behind in the cache. We need to ensure the changes
> to
> variables are atomic; that is, threads don’t see partial changes. Furthermore, we need to worry about protecting
> multiple threads from changing the data at the same time. Every single access to shared mutable state must be verified
> to be correct. Even if one of them is broken, the entire application is broken.

### ways to prevent shared mutability

- keep the mutable state well encapsulated and share only immutable data
- functional programming
- prevent it from design (STM and Actor-based Model)

----------------------


# Strategies For Concurrency

> 💡 all discussions below have ignored the overhead of context switching, for real scenario please put context switching
> in consideration
---

## How many threads to create?

> number of threads = number of available cores / (1 - blocking coefficient) \
> where 0 <= blocking coefficient < 1

- number of cores can be obtained by Runtime.getRuntime().availableProcessors()

### computation intensive application

- blocking coefficient ≈ 0 -> number of threads ≈ number of available cores
- having more threads than the cores actully hurts

### Ways to compute the blocking coefficient

- guess then converge by the following
    - profiling
    - java.lang.management API

---

## How to divide the problem

### two strategies

#### For fair load tasks

- divide the problems as many as the number of thread

#### For fair workload tasks

- make it fair, but most time it is hard/impossible, the dividing process itself may increase load
- divide ***far more parts than the threads*** to ***keep threads busy***
- the exact number of parts need to be test on real scenario

---
// todo:
- do not use more than one synchronization mechanisms at the same time eg: atomic variable + synchronized block
    - confusing and would offer no performance or safety benefit

- don't break down synchronized blocks too far(overheat)

- Avoid holding locks during lengthy computations or operations at risk of not completing quickly such as network or
  console I/O.

Debugging tip: For server applications, be sure to always specify the -server JVM command line switch when invoking the
JVM, even for development and testing. The server JVM performs more optimization than the client JVM, such as hoisting
variables out of a loop that are not modified in the loop; code that might appear to work in the development
environment (client JVM) can break in the deployment environment (server JVM)

Many developers fear that this(using immutable object for to hold all mutable variable and update atomically) approach
will create performance problems, but these fears are usually unwarranted.
Allocation is cheaper than you might think, and immutable objects offer additional performance advantages such as
reduced need for locking or defensive copies and reduced impact on generational garbage collection.

group multiple mutable variable to one immutable varible