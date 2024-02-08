# Sharing Object

## Visibility

### Reordering

> In the absence of synchronization, the compiler, processor, and runtime can do some downright weird things to the
> order in which operations appear to execute

> there is no guarantee that the reading thread will see a value written by another thread,
> Synchronization must be used to ensure visibility of memory writes across threads

### volatile (guarantee visibility not atomicity)

> Locking can guarantee both visibility and atomicity; volatile variables can only guarantee visibility

- compiler and runtime are put on notice that this variable is shared and that operations on it should not be
  reordered with other memory operations
- a read of a volatile variable always returns the most recent write by any thread
- volatile variable performs no locking
    - lighter weight than synchronized
- semantics of volatile are not strong enough to make the increment operation (count++) atomic

### Common Mistake

> assume that synchronization needs to be used only when writing to shared variables

- Reading data without synchronization is analogous to using the READ_UNCOMMITTED isolation level in a database
    - trade accuracy for performance
    - should be very careful since the data may end up stale forever and never update, causing excepted result, error
      eg: infinite loop etc

> data integrity is not guarantee for 64 bits numeric variable (double and long) without synchronization

- 64 bits numeric JVM is permitted to treat a 64-bit read or write as two separate 32-bit operations. If the reads and
  writes occur in different threads, it is therefore possible to read a nonvolatile long and get back the high 32 bits
  of one value and the low 32 bits of another

----
## Publishing Object
- this escape