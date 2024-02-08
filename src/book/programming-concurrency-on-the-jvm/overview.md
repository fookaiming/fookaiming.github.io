# Overview

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
> When we have a nonfinal (mutable) field, each time a thread changes the value, we have to consider whether we have to put the change back to the memory or leave it in the registers/cache. Each time we read the field, we need to be concerned if we read the latest valid value or a stale value left behind in the cache. We need to ensure the changes to variables are atomic; that is, threads don’t see partial changes. Furthermore, we need to worry about protecting multiple threads from changing the data at the same time. Every single access to shared mutable state must be verified to be correct. Even if one of them is broken, the entire application is broken.

### ways to prevent shared mutability
- keep the mutable state well encapsulated and share only immutable data
- functional programming
- prevent it from design (STM and Actor-based Model)

## Concurrency Model
> This book introduce three separate concurrency solutions to design concurrent programme

- [Synchronization model](./book/programming-concurrency-on-the-jvm/synchronization-model.md)
- [Sofeware Transactional Memory Model](./book/programming-concurrency-on-the-jvm/strategies-for-concurrency.md)
- [Actor-based Concurrency Model](./book/programming-concurrency-on-the-jvm/actor-based-concurrency.md)

