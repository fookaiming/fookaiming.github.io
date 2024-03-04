# Tips and Tricks

## Singleton

- one of the famous interview question is to create a thread safe singleton

### Examples

```java
public class Singleton {

    private Singleton() {
    }

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

- Many people think the above example is wrong as it may create multiple instance when it is access by multiple threads
- actually, it is safe to use if you can ensure it is created in startup thread and before all other threads is created, so it won't have
      visibility problem

### Other Examples

```java
final class Singleton {

    private static Singleton instance = new Singleton();
    
    private Singleton() { }
    
    public static Singleton getInstance() {
        return instance;
    }
}
```

- easy to implement, easy to reasoning, preferred way

```java
public class Singleton {
    private volatile static Singleton singleton;

    private Singleton() {
    }

    public static Singleton getSingleton() {
        if (singleton == null) {
            synchronized (Singleton.class) {
                if (singleton == null) {
                    singleton = new Singleton();
                }
            }
        }
        return singleton;
    }
}
```

- poor performance as volatile is used, and hard to reasoning

```java
public class Singleton {
    private Singleton() {
    }

    private static class SingletonHolder {
        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {
        return SingletonHolder.INSTANCE;
    }
}
```

- complicated inner class, hard to implement and reasoning

___

## Record

- handy way to create **_immutable_** class
- generate at compile time
- all fields are public final

### Example

```java
public record CustomerIdAndName(String id, String name) {
}

// decompiled from .class file
public record CustomerIdAndName(String id, String name) {
    public CustomerIdAndName(String id, String name) {
        this.id = id;
        this.name = name;
    }

    public String id() {
        return this.id;
    }

    public String name() {
        return this.name;
    }
}
```

### Usage

- wrap around to return multiple variables in a function call
    - prevent overuse, better use within a class to keep function simple

```java
public CustomerIdAndName getCustomerIdAndName() {
    // ...
    return new CustomerIdAndName(id, name)
}

public record CustomerIdAndName(String id, String name) {
}
```

- cleaner code

```java
    private static class ArchiveTask implements Callable<Integer> {

    private final LocalDate from;

    private final LocalDate to;

    private final ArchiveService<DailyStats> archiveService;

    private ArchiveTask(LocalDate from, LocalDate to, ArchiveService<DailyStats> archiveService) {
        this.from = from;
        this.to = to;
        this.archiveService = archiveService;
    }

    @Override
    public Integer call() {
        archiveService.archive(from, to);
        return 0;
    }
}

// much cleaner
private record ArchiveTask(LocalDate from, LocalDate to,
                           ArchiveService<DailyStats> archiveService) implements Callable<Integer> {

    @Override
    public Integer call() {
        archiveService.archive(from, to);
        return 0;
    }
}
```

___

## Collection

### create HashMap/HashSet with exact size leads to poor performance

- Hash set/map is sparse data structure
- for large data set, setting exact size induce risk of hash collision and resize may occur frequently

```java
      List<String> list = ....
Set<String> set = new HashMap(list.size())

// demo purpose only, this case better use Stream api
      for(
String s :list){
        set.

add(s.toLowerCase());
        }
```

- refer to [guava maps impl](https://github.com/google/guava/blob/master/guava/src/com/google/common/collect/Maps.java),
  init capacity with load factor
  0.75

```java
    public static <T, V> Map<T, V> newHashMapWithExpectedSize(int size) {
    return new HashMap<>(capacity(size));
}

private static int capacity(int size) {
    int capacity;
    if (size < 3) {
        capacity = size + 1;
    } else {
        capacity = (int) ((float) size / 0.75f + 1);
    }
    return capacity;
}
```

___

## Iteration

### Use iterator instead of for i loop whenever possible

- for i loop calls size() for every iteration -> poor performance
- for i loop is much slower when collection do not support RandomAccess (constant time access) eg: LinkedList
- for i loop do not support removal operation while looping non thread safe collection
