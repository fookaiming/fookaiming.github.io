# Tips and Tricks

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
public CustomerIdAndName getCustomerIdAndName(){
  // ...
  return new CustomerIdAndName(id,name)
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
    private record ArchiveTask(LocalDate from, LocalDate to, ArchiveService<DailyStats> archiveService) implements Callable<Integer> {

        @Override
            public void call() {
                archiveService.archive(from, to);
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
      for (String s : list) {
        set.add(s.toLowerCase());
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
