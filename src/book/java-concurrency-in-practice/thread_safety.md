# Thread Safety

> The core of thread safety is **managing access** to **_shared_**, _**mutable state**_

## What is thread safe class

- behaves correctly when accessed from multiple threads, regardless of the scheduling or interleaving of the execution
  of those threads by the runtime environment
- no additional synchronization or other coordination on the part of the calling code

## Ways to ensure thread safety?

- Don't share state
- Make it immutable
- coordinate the accessing by synchronization eg: lock, volatile variable, atomic class
- _**tolerance**_, is a good solution in some cases, eg: slightly inaccurate count of hits in a web-based service is an
  acceptable loss of accuracy

> In general, it is far easier to design a class to be thread-safe than to retrofit it for thread safety later

---

## Race Condition

> A race condition occurs when the correctness of a computation depends on the relative timing or interleaving of
> multiple threads by the runtime

### Some typical of race condition

- check-then-act
    - observe something to be true (file X doesn’t exist) and then take action based on that observation (create X); but
      in fact the observation could have become invalid between the time you observed it and the time you acted on it (
      someone else created X in the meantime), causing a problem (unexpected exception, overwritten data, file
      corruption)

```java
      // lazy initialization
      @NotThreadSafe
      public class LazyInitRace {
          private ExpensiveObject instance = null;
            
          public ExpensiveObject getInstance() {
              // check then act
              if (instance == null) instance = new ExpensiveObject(); 
                   
               return instance;
          }
      }
```

- Read-modify-write
    - resulting state is derived from the previous state, eg: count++, change the count number by incremental operation
      (need to read the current state in order to update the state)

---

## Atomicity

### Compound Actions

> The root cause of race condition is compound actions

- both check-then-act and Read-modify-write are compound actions that consists of multiple steps
- the data is staled whenever it is read
- make compound actions to atomic prevents other threads from using a variable while we’re in the middle of modifying it

> To preserve state consistency, update related state variables in a single atomic operation.

### Data Race

> subset of race condition, two or more threads access a shared memory location concurrently without synchronization,
> and at least one of the accesses is a write operation, lead to undefined behavior

---

## Tools to ensure atomicity

### Reentrant Lock

> locks are acquired on a per-thread rather than per-invocation basis, allowed to **_gain lock again_** when a thread
> already held the same lock

#### Intrinsic Lock/Monitor Lock

- intrinsic locks are reentrant
- mutexes (mutual exclusion lock)
- every Java object can implicitly act as intrinsic lock
- when using locks to coordinate access to a variable, the **_same lock(same object)_** must be used wherever that
  variable is accessed

```java
synchronized (lock) { 
// Access or modify shared state guarded by lock 
}
```
