Based on the existing hash table structure and comprehensive information on common pitfalls, here's the complete content for **`02-data-structures-and-algorithms\sets-maps\PITFALLS.md`**:

***

# 🟢 Sets & Maps Pitfalls: Common Mistakes & How to Avoid Them

## 1. Hash Function Design Pitfalls

### a. Poor Hash Function Leading to Excessive Collisions

**Problem:**
A bad hash function assigns many different items to the same location, causing collisions that degrade performance from O(1) to O(n).[1]

**Example:**
```text
// BAD: Poor distribution
FUNCTION badHash(key)
    RETURN key % 10  // Only 10 buckets; high collision rate
END FUNCTION

// BAD: Ignores most of string
FUNCTION badStringHash(str)
    RETURN ASCII(str[0]) % size  // Only uses first character
END FUNCTION

// GOOD: Better distribution
FUNCTION goodStringHash(str)
    hash = 5381
    FOR char IN str DO
        hash = ((hash << 5) + hash) + ASCII(char)
    END FOR
    RETURN hash % size
END FUNCTION
```

**Consequence:**
Too many collisions clump data together, making operations much slower. In worst case, performance degrades to O(n).[1]

**Solution:**
- Use well-tested hash functions (DJB2, FNV-1a, MurmurHash)
- Rely on language built-in implementations
- Ensure uniform distribution across buckets
- Test with real data patterns

---

### b. Non-Deterministic Hash Functions

**Problem:**
Hash function produces different results for same input across invocations.

**Example:**
```text
// BAD: Uses random values
FUNCTION badHash(key)
    RETURN (key + RANDOM()) % size  // Different each time!
END FUNCTION

// BAD: Uses memory address (can change)
FUNCTION badObjectHash(obj)
    RETURN obj.memoryAddress() % size  // Address-based
END FUNCTION
```

**Consequence:**
- Cannot find previously inserted keys
- Data corruption and loss
- Unpredictable behavior

**Solution:**
```text
// GOOD: Deterministic, based on content
FUNCTION goodObjectHash(obj)
    hash = 17
    hash = 31 * hash + hash(obj.field1)
    hash = 31 * hash + hash(obj.field2)
    RETURN hash % size
END FUNCTION
```

***

## 2. Load Factor and Resizing Pitfalls

### a. Ignoring Load Factor

**Problem:**
Not monitoring load factor leads to performance degradation.[1]

**Example:**
```text
// BAD: Never resize
CLASS BadHashTable
    FUNCTION put(key, value)
        idx = hash(key) % size
        buckets[idx].append((key, value))
        // No resizing; load factor unchecked
    END FUNCTION
END CLASS
```

**Analogy:**
"Think of a parking lot. When it's nearly empty, finding a spot is quick. When it's almost full, you have to drive around much longer".[1]

**Consequence:**
- High load factors increase collision probability
- Operations slow down dramatically
- Cache misses increase

**Solution:**
```text
// GOOD: Monitor and resize
CLASS GoodHashTable
    loadFactorThreshold = 0.75
    
    FUNCTION put(key, value)
        // ... insert ...
        IF (count + 1) / size > loadFactorThreshold THEN
            resize(size * 2)
        END IF
    END FUNCTION
END CLASS
```

**Best Practice:**
Keep load factor between 0.5-0.75 for optimal performance.[1]

***

### b. Inefficient Resizing

**Problem:**
Resizing entire table at once causes performance spikes.

**Example:**
```text
// BAD: Blocks for long time
FUNCTION resize(newSize)
    newBuckets = ARRAY[newSize]
    FOR bucket IN oldBuckets DO
        FOR entry IN bucket DO
            rehash(entry)  // All at once; GC spike
        END FOR
    END FOR
    buckets = newBuckets
END FUNCTION
```

**Consequence:**
- Application freezes during resize
- Large GC pauses
- Poor user experience

**Solution:**
```text
// GOOD: Incremental resizing
CLASS IncrementalHashTable
    resizingInProgress = false
    
    FUNCTION put(key, value)
        IF resizingInProgress THEN
            rehashOneBucket()  // Spread work over time
        END IF
        // ... normal insertion ...
    END FUNCTION
END CLASS
```

***

## 3. Key-Related Pitfalls

### a. Using Mutable Keys

**Problem:**
Modifying a key after insertion makes it unfindable.[5][1]

**Example:**
```python
# BAD: Mutable key (Python)
my_list = [1, 2, 3]
my_dict = {my_list: 'value'}  # TypeError!

# Even worse scenario
class MutableKey:
    def __init__(self, value):
        self.value = value
    
    def __hash__(self):
        return hash(self.value)

key = MutableKey(10)
my_dict[key] = 'data'
key.value = 20  # Changed! Now unfindable
result = my_dict[key]  # Wrong hash; won't find
```

**Explanation:**
"If you insert an item with a key and then change the key in a way that alters its hash value, the hash table won't be able to find it anymore. The item becomes effectively lost".[1]

**Consequence:**
- Data becomes inaccessible
- Unpredictable behavior
- Silent bugs difficult to debug

**Solution:**
```python
# GOOD: Immutable keys
my_tuple = (1, 2, 3)  # Tuple is immutable
my_dict = {my_tuple: 'value'}  # Works!

# GOOD: Freeze mutable objects
frozen_set = frozenset([1, 2, 3])
my_dict[frozen_set] = 'value'
```

**Best Practice:**
Use immutable types as keys: strings, numbers, tuples of immutables.[1]

---

### b. Null Keys in Languages That Don't Support Them

**Problem:**
Attempting to use null as a key when unsupported.[5]

**Example:**
```java
// Java HashMap supports null keys
HashMap<String, Integer> map = new HashMap<>();
map.put(null, 1);  // Works in Java

// But some implementations don't
ConcurrentHashMap<String, Integer> concurrentMap = new ConcurrentHashMap<>();
concurrentMap.put(null, 1);  // NullPointerException!
```

**Consequence:**
- Runtime errors
- Program crashes
- Unexpected behavior

**Solution:**
```text
// Check for null before insertion
FUNCTION put(key, value)
    IF key == null THEN
        IF NOT supportsNullKeys THEN
            THROW "Null keys not supported"
        END IF
    END IF
    // ... normal insertion ...
END FUNCTION
```

***

### c. Objects Without Proper equals() and hashCode()

**Problem:**
Using custom objects as keys without implementing hash/equality correctly.

**Example:**
```java
// BAD: No hashCode/equals override
class Person {
    String name;
    int age;
    // No hashCode or equals!
}

Person p1 = new Person("Alice", 30);
Person p2 = new Person("Alice", 30);

map.put(p1, "data");
map.get(p2);  // null! Different objects, different hashes
```

**Consequence:**
- Duplicate keys treated as different
- Cannot retrieve values
- Memory leaks (duplicate storage)

**Solution:**
```java
// GOOD: Override both methods
class Person {
    String name;
    int age;
    
    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
    
    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (!(obj instanceof Person)) return false;
        Person other = (Person) obj;
        return age == other.age && Objects.equals(name, other.name);
    }
}
```

**Contract:**
If `a.equals(b)` then `a.hashCode() == b.hashCode()` must be true.

***

## 4. Collision Resolution Pitfalls

### a. Ineffective Collision Handling

**Problem:**
Choosing inappropriate collision resolution for use case.[1]

**Example:**
```text
// BAD: Linked list with many collisions
CLASS BadChaining
    buckets = ARRAY[small_size]  // Too small
    
    FUNCTION put(key, value)
        idx = hash(key) % small_size
        buckets[idx].append((key, value))
        // Chain grows long; O(n) search
    END FUNCTION
END CLASS
```

**Consequence:**
- Some strategies use more memory (chaining)
- Others lead to clustering (linear probing)
- Poor cache locality

**Solution:**
```text
// Combine strategies
CLASS HybridHashTable
    FUNCTION put(key, value)
        IF chainLength(bucket) > threshold THEN
            convertToOpenAddressing()
        END IF
    END FUNCTION
END CLASS
```

***

### b. Tombstone Accumulation in Open Addressing

**Problem:**
Deleted entries marked with tombstones accumulate, degrading performance.

**Example:**
```text
// BAD: Never clean tombstones
FUNCTION remove(key)
    idx = hash(key) % size
    WHILE buckets[idx] != null DO
        IF buckets[idx].key == key THEN
            buckets[idx] = TOMBSTONE  // Mark deleted
            RETURN
        END IF
        idx = (idx + 1) % size
    END WHILE
END FUNCTION
```

**Consequence:**
- Tombstones treated as occupied during probing
- Search time increases
- Space wasted

**Solution:**
```text
// GOOD: Periodic cleanup
CLASS OptimizedOpenAddressing
    tombstoneCount = 0
    
    FUNCTION remove(key)
        // ... mark as tombstone ...
        tombstoneCount += 1
        
        IF tombstoneCount > size * 0.2 THEN
            cleanup()  // Rebuild without tombstones
        END IF
    END FUNCTION
END CLASS
```

***

## 5. Concurrency Pitfalls

### a. Race Conditions in Multi-threaded Access

**Problem:**
Multiple threads modifying hash table simultaneously without synchronization.

**Example:**
```text
// BAD: No synchronization
CLASS UnsafeHashTable
    FUNCTION put(key, value)
        idx = hash(key) % size
        IF buckets[idx] == null THEN
            buckets[idx] = []  // RACE: Another thread might set this
        END IF
        buckets[idx].append((key, value))  // RACE: List corruption
    END FUNCTION
END CLASS
```

**Consequence:**
- Data corruption
- Lost updates
- Crashes from corrupted data structures

**Solution:**
```text
// GOOD: Use thread-safe implementations
// Java
ConcurrentHashMap<K, V> map = new ConcurrentHashMap<>();

// Or manual synchronization
CLASS SynchronizedHashTable
    lock = LOCK()
    
    FUNCTION put(key, value)
        ACQUIRE lock
        // ... safe operations ...
        RELEASE lock
    END FUNCTION
END CLASS
```

***

### b. Deadlocks with Nested Locks

**Problem:**
Multiple hash tables with cross-referencing locks.

**Example:**
```text
// BAD: Potential deadlock
Thread 1:
    lock(mapA)
    lock(mapB)  // Waiting for Thread 2
    
Thread 2:
    lock(mapB)
    lock(mapA)  // Waiting for Thread 1
    // DEADLOCK!
```

**Solution:**
```text
// GOOD: Consistent lock ordering
Thread 1 & 2:
    IF addressOf(mapA) < addressOf(mapB) THEN
        lock(mapA)
        lock(mapB)
    ELSE
        lock(mapB)
        lock(mapA)
    END IF
```

***

## 6. Memory and Performance Pitfalls

### a. Not Pre-sizing for Known Capacity

**Problem:**
Starting with default small size when expected capacity is known.

**Example:**
```java
// BAD: Multiple resizes
HashMap<String, Integer> map = new HashMap<>();  // Default 16
for (int i = 0; i < 1000; i++) {
    map.put("key" + i, i);  // Resizes multiple times
}

// GOOD: Pre-size
HashMap<String, Integer> map = new HashMap<>(1500);  // 1000 / 0.75
for (int i = 0; i < 1000; i++) {
    map.put("key" + i, i);  // No resizing
}
```

**Consequence:**
- Multiple expensive resize operations
- Temporary memory spikes
- Slower initialization

***

### b. Memory Leaks from Unremoved Entries

**Problem:**
Not removing entries, holding references to large objects.

**Example:**
```java
// BAD: Cache never evicts
class Cache {
    Map<String, LargeObject> cache = new HashMap<>();
    
    void put(String key, LargeObject obj) {
        cache.put(key, obj);  // Never removed!
    }
}
```

**Consequence:**
- Memory grows unbounded
- OutOfMemoryError
- GC pressure

**Solution:**
```java
// GOOD: Bounded cache with eviction
class LRUCache {
    LinkedHashMap<String, LargeObject> cache = new LinkedHashMap<>(16, 0.75f, true) {
        protected boolean removeEldestEntry(Map.Entry eldest) {
            return size() > MAX_ENTRIES;
        }
    };
}

// Or use WeakHashMap for automatic cleanup
WeakHashMap<Object, Data> cache = new WeakHashMap<>();
```

***

## 7. Iterator and Modification Pitfalls

### a. Concurrent Modification During Iteration

**Problem:**
Modifying hash table while iterating over it.

**Example:**
```java
// BAD: ConcurrentModificationException
Map<String, Integer> map = new HashMap<>();
map.put("a", 1);
map.put("b", 2);

for (String key : map.keySet()) {
    if (key.equals("a")) {
        map.remove(key);  // Exception!
    }
}
```

**Consequence:**
- ConcurrentModificationException
- Undefined iteration behavior
- Skipped or duplicated elements

**Solution:**
```java
// GOOD: Use iterator.remove()
Iterator<String> it = map.keySet().iterator();
while (it.hasNext()) {
    String key = it.next();
    if (key.equals("a")) {
        it.remove();  // Safe
    }
}

// Or collect keys first
List<String> toRemove = new ArrayList<>();
for (String key : map.keySet()) {
    if (condition(key)) {
        toRemove.add(key);
    }
}
toRemove.forEach(map::remove);
```

***

### b. Assuming Iteration Order

**Problem:**
Relying on iteration order in unordered hash structures.

**Example:**
```text
// BAD: Assumes order
HashMap<String, Integer> map = new HashMap<>();
map.put("a", 1);
map.put("b", 2);
map.put("c", 3);

for (String key : map.keySet()) {
    // Expecting a, b, c order - WRONG!
}
```

**Consequence:**
- Inconsistent behavior across JVM versions
- Different results on different platforms
- Flaky tests

**Solution:**
```java
// GOOD: Use ordered structure if order matters
LinkedHashMap<String, Integer> map = new LinkedHashMap<>();
// Or TreeMap for sorted order
TreeMap<String, Integer> map = new TreeMap<>();
```

***

## 8. Type and Generics Pitfalls

### a. Raw Types Without Generics

**Problem:**
Using raw types loses type safety.

**Example:**
```java
// BAD: Raw type
HashMap map = new HashMap();
map.put("key", 123);
map.put(456, "value");  // Mixed types!

String value = (String) map.get("key");  // ClassCastException!
```

**Solution:**
```java
// GOOD: Use generics
HashMap<String, Integer> map = new HashMap<>();
map.put("key", 123);
// map.put(456, "value");  // Compile error - caught early!
```

***

### b. Type Erasure Issues

**Problem:**
Cannot create arrays of generic hash maps due to type erasure.

**Example:**
```java
// BAD: Generic array creation
Map<String, Integer>[] arrayOfMaps = new HashMap<String, Integer>[10];  // Error!
```

**Solution:**
```java
// GOOD: Use list or suppress warnings carefully
@SuppressWarnings("unchecked")
Map<String, Integer>[] arrayOfMaps = (Map<String, Integer>[]) new HashMap[10];

// Better: Use ArrayList
List<Map<String, Integer>> listOfMaps = new ArrayList<>();
```

***

## 9. Set-Specific Pitfalls

### a. Duplicate Detection Logic Errors

**Problem:**
Assuming set prevents all duplicates without proper equals().

**Example:**
```java
// BAD: No equals override
class Point {
    int x, y;
    // No equals or hashCode!
}

Set<Point> set = new HashSet<>();
set.add(new Point(1, 2));
set.add(new Point(1, 2));  // Different objects; both added!
System.out.println(set.size());  // 2, not 1!
```

**Solution:**
```java
// GOOD: Proper equals and hashCode
class Point {
    int x, y;
    
    @Override
    public boolean equals(Object o) {
        if (!(o instanceof Point)) return false;
        Point p = (Point) o;
        return x == p.x && y == p.y;
    }
    
    @Override
    public int hashCode() {
        return Objects.hash(x, y);
    }
}
```

***

### b. Set Operations with Different Types

**Problem:**
Using set operations on incompatible types.

**Example:**
```text
// BAD: Different element types
Set<Integer> setA = {1, 2, 3}
Set<String> setB = {"1", "2", "3"}

intersection = setA.intersect(setB)  // Meaningless; empty result
```

**Solution:**
```text
// GOOD: Ensure compatible types
Set<Integer> setA = {1, 2, 3}
Set<Integer> setB = {2, 3, 4}

intersection = setA.intersect(setB)  // {2, 3}
```

***

## 10. Common Edge Cases

### a. Empty Hash Table Operations

**Example:**
```text
// Edge case: Get from empty
map = {}
value = map.get("nonexistent")  // Should return null/default

// Edge case: Remove from empty
map.remove("key")  // Should handle gracefully
```

***

### b. Single Element Operations

**Example:**
```text
// Edge case: Resize with one element
map = {capacity: 1}
map.put("a", 1)
map.put("b", 2)  // Must resize from 1 → 2
```

***

### c. Integer Overflow in Hash Computation

**Problem:**
Hash computation overflows for large integers.

**Example:**
```text
// BAD: Overflow
FUNCTION hash(key)
    RETURN key * large_prime % size  // Overflow!
END FUNCTION

// GOOD: Handle overflow
FUNCTION hash(key)
    RETURN (key * large_prime_long) % size
END FUNCTION
```

***

## 11. Testing Pitfalls

### a. Not Testing Collision Scenarios

**Problem:**
Only testing with well-distributed keys.

**Solution:**
```text
// Test with intentional collisions
testKeys = ["abc", "bca", "cab"]  // Same hash
FOR key IN testKeys DO
    map.put(key, value)
END FOR
// Verify all retrievable
```

***

### b. Not Testing at Boundaries

**Problem:**
Missing tests at load factor thresholds.

**Solution:**
```text
// Test right before resize
map = HashMap(capacity=16, loadFactor=0.75)
FOR i = 1 TO 11 DO  // Just before 12th (0.75 * 16)
    map.put(i, i)
END FOR
assert map.capacity == 16

map.put(12, 12)  // Triggers resize
assert map.capacity == 32
```

***

## 12. Best Practices Summary

1. **Hash Functions:**
   - Use proven hash functions
   - Ensure deterministic behavior
   - Test distribution quality

2. **Keys:**
   - Always use immutable keys
   - Override hashCode() and equals() together
   - Check for null when necessary

3. **Load Factor:**
   - Monitor and maintain 0.5-0.75
   - Pre-size when capacity known
   - Handle resizing gracefully

4. **Concurrency:**
   - Use thread-safe implementations
   - Avoid nested locks
   - Prevent concurrent modifications

5. **Memory:**
   - Implement eviction policies
   - Use appropriate initial capacity
   - Clean up tombstones

6. **Testing:**
   - Test collision scenarios
   - Test boundary conditions
   - Test concurrent access patterns

***

**Avoiding these pitfalls transforms hash tables from potential performance nightmares into the lightning-fast data structures they're meant to be. Master these lessons to build robust, efficient, and bug-free sets and maps in production systems!**[4][5][1]

[1](https://youcademy.org/hash-table-best-practices/)
[2](https://www.reddit.com/r/programming/comments/69z19/problems_with_hash_tables/)
[3](https://dev.to/masakifukunishi/understanding-hash-tables-features-pros-cons-time-complexity-and-implementation-2nm)
[4](https://www.linkedin.com/advice/0/what-most-common-hash-table-pitfalls-how-can-you-wxkrc)
[5](https://interviewing.io/hash-tables-interview-questions)
[6](https://news.ycombinator.com/item?id=1040202)
[7](https://heycoach.in/blog/dynamic-hash-table-implementation-in-c/)
[8](https://www.geeksforgeeks.org/dsa/hashing-data-structure/)