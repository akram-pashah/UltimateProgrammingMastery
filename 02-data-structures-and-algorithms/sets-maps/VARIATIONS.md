# 🟢 Set & Map Variations: Types, Structures & Real-World Applications

## 1. Set Variations

### a. Hash Set (Unordered Set)

**Definition:**
Fast set implementation using hash table; elements stored via hash function.[1][2]

**Properties:**
- No duplicate elements[2]
- Unordered (no guaranteed iteration order)[3]
- O(1) average add/remove/contains[1][3]
- Load factor typically 0.75[1]

**Pseudocode:**
```text
CLASS HashSet
    hashTable = HashMap()  // Internal storage
    
    FUNCTION add(element)
        RETURN hashTable.put(element, true) == null
    END FUNCTION
    
    FUNCTION remove(element)
        RETURN hashTable.remove(element) != null
    END FUNCTION
    
    FUNCTION contains(element)
        RETURN hashTable.containsKey(element)
    END FUNCTION
END CLASS
```

**Language Implementations:**
- Java: `HashSet`[3][1]
- Python: `set`[1]
- C++: `std::unordered_set`[1]
- C#: `HashSet<T>`[1]

**When to Use:**
- Fast membership testing critical[3]
- Order doesn't matter[3]
- General-purpose set operations[3]

***

### b. Tree Set (Sorted Set)

**Definition:**
Ordered set implementation using self-balancing BST (Red-Black Tree).[3][1]

**Properties:**
- Elements sorted in natural or custom order[3]
- O(log n) add/remove/contains[1][3]
- Supports range queries (first, last, higher, lower)[3]
- No duplicates[3]

**Pseudocode:**
```text
CLASS TreeSet
    redBlackTree = RedBlackTree()
    
    FUNCTION add(element)
        RETURN redBlackTree.insert(element)
    END FUNCTION
    
    FUNCTION remove(element)
        RETURN redBlackTree.delete(element)
    END FUNCTION
    
    FUNCTION first()
        RETURN redBlackTree.minimum()
    END FUNCTION
    
    FUNCTION last()
        RETURN redBlackTree.maximum()
    END FUNCTION
    
    FUNCTION subSet(fromElement, toElement)
        RETURN redBlackTree.rangeQuery(fromElement, toElement)
    END FUNCTION
END CLASS
```

**Language Implementations:**
- Java: `TreeSet`[1][3]
- C++: `std::set`[1]
- C#: `SortedSet<T>`[1]

**When to Use:**
- Need sorted order[3]
- Range queries required[3]
- Predictable O(log n) performance needed[3]

**Performance Note:**
"HashSet is much faster than TreeSet (constant time versus log time for most operations)"[3]

---

### c. Linked Hash Set (Insertion-Ordered Set)

**Definition:**
Hash set maintaining insertion order via doubly-linked list.[1][3]

**Properties:**
- O(1) average operations like HashSet[3]
- Insertion-order iteration[3]
- Slightly slower than HashSet due to pointer overhead[3]

**Structure:**
```text
HashMap + Doubly-Linked List
Elements: head ↔ elem1 ↔ elem2 ↔ ... ↔ tail
```

**Pseudocode:**
```text
CLASS LinkedHashSet
    hashTable = HashMap()
    head = Node(null)
    tail = Node(null)
    
    FUNCTION add(element)
        IF hashTable.containsKey(element) THEN
            RETURN false
        END IF
        
        node = Node(element)
        hashTable.put(element, node)
        addToTail(node)
        RETURN true
    END FUNCTION
    
    FUNCTION iterator()
        current = head.next
        WHILE current != tail DO
            YIELD current.element
            current = current.next
        END WHILE
    END FUNCTION
END CLASS
```

**Language Implementations:**
- Java: `LinkedHashSet`[1][3]

**When to Use:**
- Need predictable iteration order[3]
- Debugging/logging where order matters[3]
- "Intermediate between HashSet and TreeSet"[3]

***

### d. Enum Set (Specialized)

**Definition:**
High-performance set for enum types; internally represented as bit vector.[3]

**Properties:**
- Extremely fast (bit operations)[3]
- All members must be same enum type[3]
- Supports range iteration[3]

**Example:**
```java
EnumSet<DayOfWeek> weekdays = EnumSet.range(MONDAY, FRIDAY);
EnumSet<Style> styles = EnumSet.of(BOLD, ITALIC);
```

**When to Use:**
- Working with enums exclusively[3]
- Need maximum performance[3]
- Type-safe replacement for bit flags[3]

***

### e. Copy-on-Write Array Set

**Definition:**
Thread-safe set backed by copy-on-write array.[3]

**Properties:**
- All mutations create new array copy[3]
- No locking required[3]
- Iteration concurrent with modifications[3]
- O(n) add/remove/contains[3]

**When to Use:**
- Rarely modified but frequently iterated[3]
- Event handler lists preventing duplicates[3]
- Thread-safe read-heavy workloads[3]

***

### f. Concurrent Skip List Set

**Definition:**
Thread-safe sorted set using skip list structure.

**Properties:**
- Lock-free implementation
- O(log n) operations
- Maintains sorted order
- Highly concurrent scalability

**Language Implementations:**
- Java: `ConcurrentSkipListSet`

**When to Use:**
- High-concurrency environments
- Sorted set with many writers

***

## 2. Map Variations

### a. Hash Map (Unordered Map)

**Definition:**
Fast key-value storage using hash table.[2][1]

**Properties:**
- O(1) average put/get/remove[2][1]
- Unordered keys[2]
- Unique keys, duplicate values allowed[2]
- Load factor typically 0.75[1]

**Language Implementations:**
- Java: `HashMap`[1]
- Python: `dict`[1]
- C++: `std::unordered_map`[1]
- C#: `Dictionary<K,V>`[1]
- JavaScript: `Map`[4][5]

**When to Use:**
- Fast key-based lookup critical[2]
- Order doesn't matter[2]
- General-purpose key-value storage[2]

---

### b. Tree Map (Sorted Map)

**Definition:**
Ordered map using self-balancing BST (Red-Black Tree).[2][1]

**Properties:**
- Keys sorted in natural or custom order[2]
- O(log n) put/get/remove[2][1]
- Range queries (firstKey, lastKey, subMap)[2]
- More predictable performance than HashMap[2]

**Language Implementations:**
- Java: `TreeMap`[1]
- C++: `std::map`[1]
- C#: `SortedDictionary<K,V>`[1]

**When to Use:**
- Need sorted key order[2]
- Range queries on keys[2]
- Predictable O(log n) performance[2]

---

### c. Linked Hash Map (Insertion-Ordered Map)

**Definition:**
Hash map maintaining insertion or access order via linked list.[1]

**Properties:**
- O(1) average operations[1]
- Insertion-order or access-order iteration[1]
- Can be configured for LRU cache behavior[1]

**Modes:**
- **Insertion order:** Default; elements iterated in insertion order
- **Access order:** Elements iterated by last-access time (LRU)

**Language Implementations:**
- Java: `LinkedHashMap`[1]

**When to Use:**
- Predictable iteration order needed[1]
- LRU cache implementation[1]
- Preserving insertion sequence[1]

---

### d. Multimap (Multiple Values per Key)

**Definition:**
Map allowing multiple values for single key.[6][1]

**Structure:**
```text
key1 → [value1, value2, value3]
key2 → [value4]
key3 → [value5, value6]
```

**Operations:**
```text
put(key, value): Add value to key's collection
get(key): Return all values for key
removeKey(key): Remove key and all values
removeValue(key, value): Remove specific value
```

**Implementation:**
```text
CLASS Multimap
    map = HashMap()  // key → List<value>
    
    FUNCTION put(key, value)
        IF NOT map.containsKey(key) THEN
            map.put(key, [])
        END IF
        map.get(key).append(value)
    END FUNCTION
END CLASS
```

**Language Implementations:**
- Guava (Java): `Multimap` interface[1]
- Apache Commons: `MultiValuedMap`[1]

**When to Use:**
- Grouping related items[1]
- Graph adjacency lists[1]
- One-to-many relationships[6]

***

### e. Bidirectional Map (BiMap)

**Definition:**
Maintains one-to-one mapping in both directions.[1]

**Structure:**
```text
Forward: key1 ↔ value1
         key2 ↔ value2
Reverse: value1 ↔ key1
         value2 ↔ key2
```

**Properties:**
- Keys unique, values unique (bijection)
- O(1) lookup in both directions
- Enforces one-to-one constraint

**Pseudocode:**
```text
CLASS BiMap
    forward = HashMap()
    reverse = HashMap()
    
    FUNCTION put(key, value)
        // Remove existing mappings
        IF forward.containsKey(key) THEN
            oldValue = forward.get(key)
            reverse.remove(oldValue)
        END IF
        IF reverse.containsKey(value) THEN
            oldKey = reverse.get(value)
            forward.remove(oldKey)
        END IF
        
        forward.put(key, value)
        reverse.put(value, key)
    END FUNCTION
    
    FUNCTION inverse()
        inverseBiMap = BiMap()
        inverseBiMap.forward = THIS.reverse
        inverseBiMap.reverse = THIS.forward
        RETURN inverseBiMap
    END FUNCTION
END CLASS
```

**Language Implementations:**
- Guava (Java): `BiMap` interface[1]

**When to Use:**
- ID ↔ Username mappings[1]
- Country ↔ Capital lookups[1]
- Bidirectional translations[1]

***

### f. Weak Hash Map

**Definition:**
Map with weak references to keys; keys eligible for garbage collection.

**Properties:**
- Keys can be garbage collected when no strong references exist
- Automatic entry removal when key collected
- Useful for caches and metadata storage

**Language Implementations:**
- Java: `WeakHashMap`
- JavaScript: `WeakMap`[5]

**When to Use:**
- Cache without preventing GC
- Metadata associated with objects
- Temporary associations

***

### g. Concurrent Hash Map

**Definition:**
Thread-safe hash map optimized for high concurrency.

**Properties:**
- Lock-free reads
- Segmented locking for writes
- O(1) average operations
- No global lock

**Language Implementations:**
- Java: `ConcurrentHashMap`[7]
- .NET: `ConcurrentDictionary<K,V>`

**When to Use:**
- Multi-threaded environments
- High-concurrency read/write workloads
- Avoiding synchronized overhead

***

## 3. Specialized Data Structures

### a. Multiset (Bag)

**Definition:**
Set allowing duplicate elements; tracks element frequency.[1]

**Structure:**
```text
Multiset: ["apple", "apple", "banana", "apple"]
Internal: {"apple": 3, "banana": 1}
```

**Operations:**
```text
add(element): Increment count
remove(element): Decrement count
count(element): Return frequency
totalCount(): Sum of all frequencies
```

**Pseudocode:**
```text
CLASS Multiset
    map = HashMap()  // element → count
    totalCount = 0
    
    FUNCTION add(element)
        count = map.get(element, 0)
        map.put(element, count + 1)
        totalCount += 1
    END FUNCTION
    
    FUNCTION count(element)
        RETURN map.get(element, 0)
    END FUNCTION
END CLASS
```

**Language Implementations:**
- Guava (Java): `Multiset`[1]
- Python: `collections.Counter`[1]

**When to Use:**
- Frequency counting[1]
- Histograms[1]
- Word count applications[1]

***

### b. LRU Cache (Map + Doubly-Linked List)

**Definition:**
Fixed-capacity cache evicting least-recently-used items.[1]

**Structure:**
```text
HashMap: key → Node
Doubly-Linked List: MRU ↔ ... ↔ LRU
```

**Complexity:**
- get: O(1)
- put: O(1)
- eviction: O(1)

**Pseudocode:**
```text
CLASS LRUCache
    capacity = 0
    cache = HashMap()
    head = Node(-1, -1)
    tail = Node(-1, -1)
    
    FUNCTION get(key)
        IF NOT cache.containsKey(key) THEN
            RETURN -1
        END IF
        
        node = cache.get(key)
        moveToFront(node)
        RETURN node.value
    END FUNCTION
    
    FUNCTION put(key, value)
        IF cache.containsKey(key) THEN
            node = cache.get(key)
            node.value = value
            moveToFront(node)
        ELSE
            IF cache.size() >= capacity THEN
                lru = tail.prev
                removeNode(lru)
                cache.remove(lru.key)
            END IF
            
            node = Node(key, value)
            cache.put(key, node)
            addToFront(node)
        END IF
    END FUNCTION
END CLASS
```

**Real Applications:**
- Browser caches[1]
- CPU caches[1]
- Database buffer pools[1]

***

### c. Bloom Filter (Probabilistic Set)

**Definition:**
Space-efficient probabilistic set; allows false positives, no false negatives.[1]

**Properties:**
- O(k) add/contains where k = number of hash functions[1]
- O(m) bits space where m << n*log(n)[1]
- Tunable false positive rate[1]

**Pseudocode:**
```text
CLASS BloomFilter
    bits = BITARRAY(size)
    numHashFunctions = 0
    
    FUNCTION add(element)
        FOR i = 0 TO numHashFunctions - 1 DO
            idx = hash(element, i) % size
            bits[idx] = 1
        END FOR
    END FUNCTION
    
    FUNCTION contains(element)
        FOR i = 0 TO numHashFunctions - 1 DO
            idx = hash(element, i) % size
            IF bits[idx] == 0 THEN
                RETURN false  // Definitely NOT in set
            END IF
        END FOR
        RETURN true  // Probably in set
    END FUNCTION
END FUNCTION
```

**Real Applications:**
- Spell checkers[1]
- URL duplicate detection[1]
- Cache filters[1]
- Cassandra/HBase storefile indexes[1]

***

### d. Count-Min Sketch

**Definition:**
Approximate frequency counting with sublinear space.[1]

**Properties:**
- O(ε⁻¹ log δ⁻¹) space for ε-approximate counts with probability 1-δ[1]
- O(k) add/query where k = number of hash functions
- Never underestimates; may overestimate

**Pseudocode:**
```text
CLASS CountMinSketch
    table = 2D_ARRAY[depth][width]
    hashFunctions = []
    
    FUNCTION add(element)
        FOR i = 0 TO depth - 1 DO
            idx = hashFunctions[i](element) % width
            table[i][idx] += 1
        END FOR
    END FUNCTION
    
    FUNCTION count(element)
        minCount = INFINITY
        FOR i = 0 TO depth - 1 DO
            idx = hashFunctions[i](element) % width
            minCount = MIN(minCount, table[i][idx])
        END FOR
        RETURN minCount
    END FUNCTION
END CLASS
```

**Real Applications:**
- Streaming analytics[1]
- Top-K queries[1]
- Network traffic monitoring[1]

***

### e. Consistent Hashing

**Definition:**
Hash function minimizing remapping when nodes added/removed.[1]

**Structure:**
```text
Virtual ring: Hash(server) → position on ring
Key routing: Hash(key) → next server clockwise
```

**Properties:**
- Only O(k/n) keys affected on node add/remove[1]
- Virtual nodes (replicas) improve distribution[1]
- O(log n) lookup via TreeMap[1]

**Pseudocode:**
```text
CLASS ConsistentHash
    ring = TreeMap()  // hash → node
    numReplicas = 150
    
    FUNCTION addNode(node)
        FOR i = 0 TO numReplicas - 1 DO
            hash = hashFunction(node + "_" + i)
            ring.put(hash, node)
        END FOR
    END FUNCTION
    
    FUNCTION getNode(key)
        hash = hashFunction(key)
        entry = ring.ceilingEntry(hash)
        IF entry == null THEN
            entry = ring.firstEntry()  // Wrap around
        END IF
        RETURN entry.value
    END FUNCTION
END CLASS
```

**Real Applications:**
- Memcached/Redis clusters[1]
- Load balancers[1]
- Distributed hash tables (DHTs)[1]

***

## 4. Collision Resolution Strategies

### a. Separate Chaining (Closed Addressing)

**Definition:**
Each bucket stores linked list of entries.[1]

**Pros:**
- Simple implementation[1]
- Graceful performance degradation[1]
- Easy deletion[1]

**Cons:**
- Extra memory for pointers[1]
- Poor cache locality[1]

**Used In:**
- Java HashMap[1]
- Most educational implementations[1]

***

### b. Linear Probing

**Definition:**
Probe sequence: h(key), h(key)+1, h(key)+2, ....[1]

**Pros:**
- Excellent cache locality[1]
- Simple implementation[1]

**Cons:**
- Primary clustering (collisions cause more collisions)[1]

**Used In:**
- Python dict (CPython 3.6+)[1]
- System-level hash tables[1]

***

### c. Quadratic Probing

**Definition:**
Probe sequence: h(key), h(key)+1², h(key)+4², h(key)+9², ....[1]

**Pros:**
- Reduces clustering vs linear probing[1]

**Cons:**
- Secondary clustering[1]
- Requires prime table size[1]

***

### d. Double Hashing

**Definition:**
Probe sequence: h1(key), h1(key) + d×h2(key), ....[1]

**Pros:**
- Best distribution among open addressing[1]
- Avoids both primary and secondary clustering[1]

**Cons:**
- Need two hash functions[1]

***

### e. Cuckoo Hashing

**Definition:**
Multiple hash functions; displace existing entries on collision.[1]

**Properties:**
- O(1) worst-case lookup[1]
- Amortized O(1) insertion[1]
- Higher load factor possible[1]

**Pros:**
- Guaranteed O(1) worst-case lookup[1]
- High space efficiency[1]

**Cons:**
- Complex implementation[1]
- Insertion worst-case O(n)[1]

***

### f. Robin Hood Hashing

**Definition:**
Minimize variance in probe distances by "stealing from rich".[1]

**Benefit:**
- More uniform access time[1]
- Reduces worst-case clustering[1]

**Used In:**
- Rust HashMap (previous implementation)
- Performance-critical C++ libraries[1]

***

### g. Hopscotch Hashing

**Definition:**
Keep entries near home position; limit search radius.[8][1]

**Properties:**
- Excellent cache locality[1]
- Search bounded within neighborhood[1]
- High load factor (>90%) supported[8]

**Algorithm:**
"The neighbourhood characteristic guarantees that the cost of finding the desired item from any given bucket within the neighbourhood is very close to the cost of finding it in the bucket itself".[8]

**Used In:**
- GPU hash tables[1]
- Cache-optimized systems[1]
- Concurrent hash tables[8]

***

## 5. Modern Hash Table Implementations

### a. Swiss Table (Google Abseil)

**Definition:**
Flat hash map with SIMD-optimized probing.[7]

**Properties:**
- Uses SIMD instructions for parallel bucket checking
- Metadata bytes for fast empty slot detection
- Open addressing with quadratic probing
- Excellent cache locality

**Performance:**
Significantly faster than `std::unordered_map` in most benchmarks.

**Used In:**
- Google internal services
- C++ Abseil library

***

### b. F14 (Facebook)

**Definition:**
High-performance hash table with SIMD optimizations.

**Properties:**
- SIMD-based probing
- Optimized memory layout
- Vectorized operations

**Used In:**
- Facebook internal infrastructure
- Folly library

***

### c. ClickHouse Hash Tables

**Definition:**
Specialized hash tables for database operations.[7]

**Variants:**
- **String hash tables:** 4 specialized tables for different string lengths[7]
  - 0-8 bytes
  - 9-16 bytes
  - 17-24 bytes
  - >24 bytes
- **Two-level hash tables:** 256 hash tables for multi-threaded GROUP BY[7]

**Benefit:**
"When we need to join tables, we use minimal synchronization and join them by columns. In the end, nothing slows down here."[7]

---

## 6. Hash Function Variations

### a. Simple Hash Functions

**Modulo:**
```text
h(key) = key % m
```
Problem: Poor distribution if key LSBs correlated.[1]

**Multiplicative (Knuth):**
```text
h(key) = ⌊m × (k×A mod 1)⌋ where A = (√5 - 1)/2 ≈ 0.618
```
Better distribution; less clustering.[1]

**Folding:**
```text
Divide key into parts; XOR or sum parts
```
Used for composite keys.[1]

***

### b. String Hash Functions

**Polynomial Rolling Hash:**
```text
h(s) = (s[0]×p^(n-1) + s[1]×p^(n-2) + ... + s[n-1]) mod m
p = prime (typically 31 or 37)
```

**DJB2:**
```text
h(s) = 5381
for each char c in s:
  h = ((h << 5) + h) + c
return |h| % m
```

**Cryptographic (SHA-256, MD5):**
- Excellent distribution[1]
- Collision-resistant[1]
- Slow; overkill for hash tables[1]

***

### c. Universal Hashing

**Definition:**
Randomized hash function from universal family.[1]

**Property:**
```text
For any two distinct keys k1, k2:
Pr[h(k1) = h(k2)] ≤ 1/m
```

**Used In:**
- Theoretical analysis[1]
- Load-balanced systems[1]

***

## 7. Choosing the Right Variation

| Use Case | Variant | Why |
|----------|---------|-----|
| General purpose | Hash Set/Map | Fast O(1) operations; standard [1][3] |
| Sorted order | Tree Set/Map | O(log n) with ordering guarantees [3] |
| Insertion order | Linked Hash Set/Map | Predictable iteration [3] |
| LRU cache | Hash + Doubly-linked list | O(1) all operations [1] |
| High concurrency | Concurrent Hash Map | Lock-free reads; scalable [7] |
| Memory constrained | Bloom Filter | O(m) bits vs O(n) space [1] |
| Distributed systems | Consistent Hashing | Minimal remapping [1] |
| Enum types | Enum Set | Bit vector; extremely fast [3] |
| Frequency counting | Multiset/Counter | Track duplicates [1] |
| Approximate counting | Count-Min Sketch | Sublinear space [1] |

***

## 8. Performance Comparison

| Operation | Chaining | Linear Probing | Quadratic | Double Hash | Cuckoo |
|-----------|----------|----------------|-----------|-------------|--------|
| Insert avg | O(1+α) | O(1+α) | O(1+α) | O(1+α) | O(1) amortized |
| Insert worst | O(n) | O(n) | O(n) | O(n) | O(n) |
| Search avg | O(1+α) | O(1+α) | O(1+α) | O(1+α) | O(1) worst |
| Search worst | O(n) | O(n) | O(n) | O(n) | O(1) |
| Delete avg | O(1+α) | O(1+α) tombstone | O(1+α) tombstone | O(1+α) tombstone | O(1) |
| Cache friendly | Poor | Excellent | Good | Good | Excellent |
| Max load factor | 0.75+ | 0.5 | 0.5 | 0.5 | 0.5 |

α = load factor (n/m)[1]

---

## 9. Language-Specific Implementations

### Python
```python
# Hash-based
hash_set = set()
hash_dict = dict()

# Frequency counter (multiset)
from collections import Counter
counter = Counter(['a', 'b', 'a'])

# Ordered dict
from collections import OrderedDict
ordered = OrderedDict()
```

### Java
```java
// Hash-based
HashSet<Integer> hashSet = new HashSet<>();
HashMap<String, Integer> hashMap = new HashMap<>();

// Sorted
TreeSet<Integer> treeSet = new TreeSet<>();
TreeMap<String, Integer> treeMap = new TreeMap<>();

// Insertion-ordered
LinkedHashSet<Integer> linkedSet = new LinkedHashSet<>();
LinkedHashMap<String, Integer> linkedMap = new LinkedHashMap<>();

// Concurrent
ConcurrentHashMap<String, Integer> concurrentMap = new ConcurrentHashMap<>();
```

### C++
```cpp
// Hash-based
std::unordered_set<int> hashSet;
std::unordered_map<std::string, int> hashMap;

// Sorted
std::set<int> treeSet;
std::map<std::string, int> treeMap;

// Multiset/multimap
std::unordered_multiset<int> multiset;
std::unordered_multimap<std::string, int> multimap;
```

### JavaScript
```javascript
// Modern ES6+
const set = new Set();
const map = new Map();

// Weak references (GC-friendly)
const weakSet = new WeakSet();
const weakMap = new WeakMap();
```

***

## 10. Best Practices

**Default Choice:**
- Use language's built-in hash set/map for most cases[1]
- "It's generally straightforward to decide... HashSet is much faster than TreeSet"[3]

**Collision Resolution:**
- Separate chaining for simplicity[1]
- Open addressing for cache locality[1]

**Load Factor:**
- Keep ≤ 0.75 for good performance[1]
- "Accept the default; it's almost always the right thing to do"[3]

**Initial Capacity:**
- "Pick a number that's about twice the size to which you expect the set to grow"[3]
- Avoid frequent resizing overhead

**Testing:**
- Test with high collision scenarios[1]
- Test empty, single element, boundary cases[1]

***

**Master set and map variations to choose the perfect structure for any scenario—from standard dictionaries to specialized caches to distributed systems at scale!**[4][7][2][3][1]

[1](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/87948039/000e0325-1b66-4312-948c-b024bf5448ef/VARIATIONS.md)
[2](https://www.geeksforgeeks.org/dsa/introduction-to-map-data-structure/)
[3](https://www.iitk.ac.in/esc101/05Aug/tutorial/collections/implementations/set.html)
[4](https://javascript.info/map-set)
[5](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)
[6](https://stackoverflow.com/questions/23466503/confusion-about-data-structures-list-set-and-map)
[7](https://clickhouse.com/blog/hash-tables-in-clickhouse-and-zero-cost-abstractions)
[8](https://en.wikipedia.org/wiki/Hash_table)
[9](https://www.geeksforgeeks.org/javascript/set-vs-map-in-javascript/)
[10](https://meritshot.com/blogs/data-structures-and-algorithms-roadmap-2025-what-top-developers-are-learning-right-now)
[11](https://docs.oracle.com/javase/tutorial/collections/implementations/set.html)