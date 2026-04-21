# 🟢 Sets & Maps: Comprehensive Mastery from Foundations to Advanced Systems

## 1. What are Sets and Maps?

### Sets
A **set** is an abstract data type that stores a collection of **unique** elements with no specific order. Sets emphasize membership testing, uniqueness enforcement, and mathematical set operations (union, intersection, difference).[1][2]

**Key Characteristics:**
- **No duplicates**: Each element appears at most once[2]
- **Unordered**: Elements have no inherent sequence (implementation-dependent)[3]
- **Fast membership testing**: O(1) average for hash-based, O(log n) for tree-based[2]
- **Dynamic sizing**: Grows/shrinks as elements added/removed[2]

### Maps
A **map** (also called **dictionary**, **associative array**, or **hash table**) is a data structure that stores **key-value pairs**, where each key is unique and maps to exactly one value.[4][3]

**Key Characteristics:**
- **Key uniqueness**: Each key appears at most once; values can duplicate[3]
- **Key-based retrieval**: Access values via their associated keys in O(1) average time[3]
- **Unordered** (hash-based) or **ordered** (tree-based)[3]
- **Dynamic resizing**: Automatically grows when load factor exceeds threshold[4]

### Core Relationship
Maps are the **superset** of sets: a set can be implemented as a map where keys are elements and values are dummy placeholders.[5][4]

---

## 2. Set Data Structure

### a. Set Fundamentals

**Definition:**
A set $$ S $$ is a collection of distinct elements $$ e_1, e_2, ..., e_n $$ where:
- **Uniqueness**: $$ \forall i \neq j, e_i \neq e_j $$
- **Membership**: $$ e \in S $$ or $$ e \notin S $$
- **Cardinality**: $$ |S| = n $$ (number of elements)

**Core Operations:**
```text
add(element): Add element to set (if not present)
remove(element): Remove element from set
contains(element): Check if element exists
size(): Return number of elements
isEmpty(): Check if set is empty
clear(): Remove all elements
```

### b. Set Implementations

#### Hash Set (Unordered)
Uses hash table internally; elements stored via hash function.[6][2]

**Structure:**
```text
Hash Set with m=10 buckets:
  Bucket 0: []
  Bucket 1: ["apple", "apricot"]
  Bucket 2: ["banana"]
  Bucket 3: []
  Bucket 4: ["cherry"]
  ...
```

**Complexity:**
- **Add/Remove/Contains**: O(1) average, O(n) worst-case (all collisions)
- **Space**: O(n)

**Pseudocode:**
```text
CLASS HashSet
    buckets = ARRAY[size]
    size = 16
    count = 0
    
    FUNCTION hash(element)
        hash = 0
        FOR char IN element DO
            hash += ASCII(char)
        END FOR
        RETURN hash % size
    END FUNCTION
    
    FUNCTION add(element)
        idx = hash(element)
        IF buckets[idx] == null THEN
            buckets[idx] = []
        END IF
        
        FOR item IN buckets[idx] DO
            IF item == element THEN
                RETURN false  // Already exists
            END IF
        END FOR
        
        buckets[idx].append(element)
        count += 1
        
        IF count / size > 0.75 THEN
            resize()
        END IF
        RETURN true
    END FUNCTION
    
    FUNCTION contains(element)
        idx = hash(element)
        IF buckets[idx] == null THEN
            RETURN false
        END IF
        
        FOR item IN buckets[idx] DO
            IF item == element THEN
                RETURN true
            END IF
        END FOR
        RETURN false
    END FUNCTION
    
    FUNCTION remove(element)
        idx = hash(element)
        IF buckets[idx] == null THEN
            RETURN false
        END IF
        
        FOR i = 0 TO len(buckets[idx]) - 1 DO
            IF buckets[idx][i] == element THEN
                buckets[idx].removeAt(i)
                count -= 1
                RETURN true
            END IF
        END FOR
        RETURN false
    END FUNCTION
END CLASS
```

**When to Use:**
- Fast membership testing critical
- Order doesn't matter
- Memory efficient (low overhead)

#### Tree Set (Ordered)
Uses self-balancing binary search tree (Red-Black Tree, AVL Tree).[2]

**Structure:**
```text
Tree Set (BST):
       50
      /  \
    30    70
   /  \   / \
  20  40 60 80
```

**Complexity:**
- **Add/Remove/Contains**: O(log n) guaranteed
- **Min/Max/Range queries**: O(log n)
- **Space**: O(n)

**Pseudocode:**
```text
CLASS TreeSet (Using Red-Black Tree)
    root = null
    count = 0
    
    FUNCTION add(element)
        IF root == null THEN
            root = Node(element)
            count += 1
            RETURN true
        END IF
        
        node = root
        WHILE true DO
            IF element < node.value THEN
                IF node.left == null THEN
                    node.left = Node(element)
                    count += 1
                    balanceTree()
                    RETURN true
                ELSE
                    node = node.left
                END IF
            ELSE IF element > node.value THEN
                IF node.right == null THEN
                    node.right = Node(element)
                    count += 1
                    balanceTree()
                    RETURN true
                ELSE
                    node = node.right
                END IF
            ELSE
                RETURN false  // Duplicate
            END IF
        END WHILE
    END FUNCTION
    
    FUNCTION contains(element)
        node = root
        WHILE node != null DO
            IF element < node.value THEN
                node = node.left
            ELSE IF element > node.value THEN
                node = node.right
            ELSE
                RETURN true
            END IF
        END WHILE
        RETURN false
    END FUNCTION
END CLASS
```

**When to Use:**
- Need sorted order
- Range queries (elements between x and y)
- Min/max retrieval
- Predictable O(log n) performance

#### Linked Hash Set
Maintains **insertion order** using hash table + doubly-linked list.[2]

**Structure:**
```text
Hash table + linked list:
  Bucket 2: ["apple"] <--> ["banana"] <--> ["cherry"]
  Order: apple → banana → cherry
```

**Complexity:**
- **Add/Remove/Contains**: O(1) average
- **Iteration**: O(n) in insertion order
- **Space**: O(n) + pointer overhead

**When to Use:**
- Need insertion order preservation
- Iterating in predictable sequence

### c. Set Operations

#### Mathematical Set Operations
```text
FUNCTION union(setA, setB)
    result = new Set()
    FOR element IN setA DO
        result.add(element)
    END FOR
    FOR element IN setB DO
        result.add(element)
    END FOR
    RETURN result
END FUNCTION
Complexity: O(|A| + |B|)

FUNCTION intersection(setA, setB)
    result = new Set()
    FOR element IN setA DO
        IF setB.contains(element) THEN
            result.add(element)
        END IF
    END FOR
    RETURN result
END FUNCTION
Complexity: O(|A| × time(contains))

FUNCTION difference(setA, setB)
    result = new Set()
    FOR element IN setA DO
        IF NOT setB.contains(element) THEN
            result.add(element)
        END IF
    END FOR
    RETURN result
END FUNCTION
Complexity: O(|A| × time(contains))

FUNCTION symmetricDifference(setA, setB)
    // (A - B) ∪ (B - A)
    result = difference(setA, setB)
    FOR element IN difference(setB, setA) DO
        result.add(element)
    END FOR
    RETURN result
END FUNCTION
Complexity: O(|A| + |B|)

FUNCTION isSubset(setA, setB)
    FOR element IN setA DO
        IF NOT setB.contains(element) THEN
            RETURN false
        END IF
    END FOR
    RETURN true
END FUNCTION
Complexity: O(|A| × time(contains))

FUNCTION isSuperset(setA, setB)
    RETURN isSubset(setB, setA)
END FUNCTION

FUNCTION isDisjoint(setA, setB)
    FOR element IN setA DO
        IF setB.contains(element) THEN
            RETURN false
        END IF
    END FOR
    RETURN true
END FUNCTION
Complexity: O(|A| × time(contains))
```

***

## 3. Map Data Structure

### a. Map Fundamentals

**Definition:**
A map $$ M $$ is a collection of key-value pairs $$ (k_1, v_1), (k_2, v_2), ..., (k_n, v_n) $$ where:
- **Key uniqueness**: $$ \forall i \neq j, k_i \neq k_j $$
- **Key-value binding**: Each key maps to exactly one value
- **Value duplication allowed**: Multiple keys can map to same value

**Core Operations:**
```text
put(key, value): Insert or update key-value pair
get(key): Retrieve value associated with key
remove(key): Delete key-value pair
containsKey(key): Check if key exists
containsValue(value): Check if value exists
size(): Return number of key-value pairs
isEmpty(): Check if map is empty
clear(): Remove all entries
keys(): Return set of all keys
values(): Return collection of all values
entries(): Return set of all key-value pairs
```

### b. Map Implementations

#### Hash Map (Unordered)
Hash table with key-value pairs; fast operations.[5][4]

**Structure:**
```text
Hash Map with m=5 buckets:
  Bucket 0: []
  Bucket 1: [("apple", 5), ("apricot", 3)]
  Bucket 2: [("banana", 7)]
  Bucket 3: []
  Bucket 4: [("cherry", 2)]
```

**Pseudocode:**
```text
CLASS HashMap
    buckets = ARRAY[size]
    size = 16
    count = 0
    
    FUNCTION hash(key)
        hash = 0
        FOR char IN key DO
            hash += ASCII(char)
        END FOR
        RETURN hash % size
    END FUNCTION
    
    FUNCTION put(key, value)
        idx = hash(key)
        IF buckets[idx] == null THEN
            buckets[idx] = []
        END IF
        
        // Check if key exists; update value
        FOR pair IN buckets[idx] DO
            IF pair.key == key THEN
                pair.value = value
                RETURN
            END IF
        END FOR
        
        // Key not found; add new entry
        buckets[idx].append((key, value))
        count += 1
        
        IF count / size > 0.75 THEN
            resize()
        END IF
    END FUNCTION
    
    FUNCTION get(key)
        idx = hash(key)
        IF buckets[idx] == null THEN
            RETURN null
        END IF
        
        FOR pair IN buckets[idx] DO
            IF pair.key == key THEN
                RETURN pair.value
            END IF
        END FOR
        RETURN null
    END FUNCTION
    
    FUNCTION remove(key)
        idx = hash(key)
        IF buckets[idx] == null THEN
            RETURN null
        END IF
        
        FOR i = 0 TO len(buckets[idx]) - 1 DO
            IF buckets[idx][i].key == key THEN
                value = buckets[idx][i].value
                buckets[idx].removeAt(i)
                count -= 1
                RETURN value
            END IF
        END FOR
        RETURN null
    END FUNCTION
    
    FUNCTION containsKey(key)
        RETURN get(key) != null
    END FUNCTION
    
    FUNCTION containsValue(value)
        FOR bucket IN buckets DO
            IF bucket != null THEN
                FOR pair IN bucket DO
                    IF pair.value == value THEN
                        RETURN true
                    END IF
                END FOR
            END IF
        END FOR
        RETURN false
    END FUNCTION
END CLASS
```

**Complexity:**
- **put/get/remove/containsKey**: O(1) average, O(n) worst-case
- **containsValue**: O(n) (must scan all entries)
- **Space**: O(n)

**When to Use:**
- Fast key-based access critical
- Order doesn't matter
- Keys have good hash distribution

#### Tree Map (Ordered)
Self-balancing BST storing key-value pairs; keys sorted.[3][2]

**Structure:**
```text
Tree Map (Red-Black Tree):
       ("dog", 3)
       /         \
  ("cat", 5)   ("lion", 8)
     /              \
("ant", 2)      ("zebra", 1)
```

**Complexity:**
- **put/get/remove**: O(log n) guaranteed
- **firstKey/lastKey**: O(log n)
- **Range queries**: O(log n + k) where k = result size
- **Space**: O(n)

**Pseudocode:**
```text
CLASS TreeMap (Using Red-Black Tree)
    root = null
    count = 0
    
    FUNCTION put(key, value)
        IF root == null THEN
            root = Node(key, value)
            count += 1
            RETURN
        END IF
        
        node = root
        WHILE true DO
            IF key < node.key THEN
                IF node.left == null THEN
                    node.left = Node(key, value)
                    count += 1
                    balanceTree()
                    RETURN
                ELSE
                    node = node.left
                END IF
            ELSE IF key > node.key THEN
                IF node.right == null THEN
                    node.right = Node(key, value)
                    count += 1
                    balanceTree()
                    RETURN
                ELSE
                    node = node.right
                END IF
            ELSE
                node.value = value  // Update existing
                RETURN
            END IF
        END WHILE
    END FUNCTION
    
    FUNCTION get(key)
        node = root
        WHILE node != null DO
            IF key < node.key THEN
                node = node.left
            ELSE IF key > node.key THEN
                node = node.right
            ELSE
                RETURN node.value
            END IF
        END WHILE
        RETURN null
    END FUNCTION
    
    FUNCTION firstKey()
        node = root
        WHILE node.left != null DO
            node = node.left
        END WHILE
        RETURN node.key
    END FUNCTION
    
    FUNCTION lastKey()
        node = root
        WHILE node.right != null DO
            node = node.right
        END WHILE
        RETURN node.key
    END FUNCTION
    
    FUNCTION rangeQuery(keyStart, keyEnd)
        result = []
        inOrderTraversal(root, keyStart, keyEnd, result)
        RETURN result
    END FUNCTION
    
    FUNCTION inOrderTraversal(node, start, end, result)
        IF node == null THEN
            RETURN
        END IF
        
        IF node.key > start THEN
            inOrderTraversal(node.left, start, end, result)
        END IF
        
        IF node.key >= start AND node.key <= end THEN
            result.append((node.key, node.value))
        END IF
        
        IF node.key < end THEN
            inOrderTraversal(node.right, start, end, result)
        END IF
    END FUNCTION
END CLASS
```

**When to Use:**
- Need sorted key order
- Range queries (all keys between x and y)
- Min/max key retrieval
- Predictable O(log n) performance

#### Linked Hash Map
Maintains **insertion order** or **access order** using hash table + doubly-linked list.[7][4]

**Structure:**
```text
Hash table + linked list:
  Bucket 1: ("apple", 5) <--> ("banana", 3) <--> ("cherry", 7)
  Order: apple → banana → cherry
```

**Complexity:**
- **put/get/remove**: O(1) average
- **Iteration**: O(n) in insertion/access order
- **Space**: O(n) + pointer overhead

**When to Use:**
- Need predictable iteration order
- LRU cache implementation (access order mode)
- Debugging/logging (insertion order matters)

***

## 4. Advanced Set & Map Variations

### a. Multiset (Bag)
Allows **duplicate elements**; tracks element frequency.[2]

**Structure:**
```text
Multiset: ["apple", "apple", "banana", "apple"]
Internal: {("apple", 3), ("banana", 1)}
```

**Operations:**
```text
add(element): Increment count
remove(element): Decrement count (or remove if count=0)
count(element): Return frequency
totalCount(): Sum of all frequencies
```

**Implementation:**
```text
CLASS Multiset
    map = HashMap()  // element -> count
    
    FUNCTION add(element)
        count = map.get(element, 0)
        map.put(element, count + 1)
    END FUNCTION
    
    FUNCTION remove(element)
        count = map.get(element, 0)
        IF count > 1 THEN
            map.put(element, count - 1)
        ELSE IF count == 1 THEN
            map.remove(element)
        END IF
    END FUNCTION
    
    FUNCTION count(element)
        RETURN map.get(element, 0)
    END FUNCTION
END CLASS
```

**When to Use:**
- Frequency counting (word counts, histograms)
- Tracking duplicates explicitly

### b. Multimap
Allows **multiple values per key**.[7]

**Structure:**
```text
Multimap:
  "color" -> ["red", "blue", "green"]
  "size" -> ["small", "large"]
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
    map = HashMap()  // key -> List<value>
    
    FUNCTION put(key, value)
        IF NOT map.containsKey(key) THEN
            map.put(key, [])
        END IF
        map.get(key).append(value)
    END FUNCTION
    
    FUNCTION get(key)
        RETURN map.get(key, [])
    END FUNCTION
    
    FUNCTION removeValue(key, value)
        values = map.get(key, [])
        values.remove(value)
        IF values.isEmpty() THEN
            map.remove(key)
        END IF
    END FUNCTION
END CLASS
```

**When to Use:**
- Grouping (students by grade, products by category)
- Graph adjacency lists (vertex -> [neighbors])

### c. Bidirectional Map (BiMap)
Maintains **one-to-one mapping** in both directions.[7]

**Structure:**
```text
BiMap:
  Forward: {"key1" -> "valueA", "key2" -> "valueB"}
  Reverse: {"valueA" -> "key1", "valueB" -> "key2"}
```

**Operations:**
```text
put(key, value): Add bidirectional mapping
getByKey(key): Get value by key
getByValue(value): Get key by value
removeByKey(key): Remove by key
removeByValue(value): Remove by value
```

**Implementation:**
```text
CLASS BiMap
    forward = HashMap()  // key -> value
    reverse = HashMap()  // value -> key
    
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
    
    FUNCTION getByKey(key)
        RETURN forward.get(key)
    END FUNCTION
    
    FUNCTION getByValue(value)
        RETURN reverse.get(value)
    END FUNCTION
END CLASS
```

**When to Use:**
- ID <-> Username mappings
- Country <-> Capital lookups
- Inverse indexing

### d. Sorted Set (Ordered Set)
Set maintaining **sorted order** (typically via tree).[2]

**Operations:**
```text
first(): Return minimum element
last(): Return maximum element
higher(element): Return smallest element > element
lower(element): Return largest element < element
subSet(start, end): Return elements in range
```

**When to Use:**
- Leaderboards (sorted scores)
- Time-series data (sorted timestamps)
- Range queries on sets

***

## 5. Hash Function Design for Sets & Maps

### a. Good Hash Function Properties
- **Deterministic**: Same input always produces same hash[4]
- **Uniform distribution**: Minimizes collisions; even spread[4]
- **Fast computation**: O(1) time[4]
- **Avalanche effect**: Small input change → large hash change[4]

### b. Hash Functions by Type

#### Integers
```text
FUNCTION hashInteger(key, tableSize)
    // Simple modulo
    RETURN key % tableSize
END FUNCTION

FUNCTION hashIntegerMultiplicative(key, tableSize)
    // Multiplicative hashing (Knuth)
    A = 0.6180339887  // (√5 - 1) / 2
    RETURN floor(tableSize * ((key * A) % 1))
END FUNCTION
```

#### Strings
```text
FUNCTION polynomialRollingHash(str, tableSize)
    // Polynomial rolling hash
    hash = 0
    base = 31
    power = 1
    FOR char IN str DO
        hash += (ASCII(char) * power) % tableSize
        power = (power * base) % tableSize
    END FOR
    RETURN hash % tableSize
END FUNCTION

FUNCTION djb2Hash(str, tableSize)
    // DJB2 hash function (good distribution)
    hash = 5381
    FOR char IN str DO
        hash = ((hash << 5) + hash) + ASCII(char)
    END FOR
    RETURN ABS(hash) % tableSize
END FUNCTION
```

#### Custom Objects
```text
FUNCTION hashObject(obj, tableSize)
    // Combine field hashes
    hash = 0
    hash = 31 * hash + hashField(obj.field1)
    hash = 31 * hash + hashField(obj.field2)
    ...
    RETURN hash % tableSize
END FUNCTION
```

### c. Universal Hashing
Randomized hash function family; defeats adversarial inputs.[4]

```text
FUNCTION universalHash(key, tableSize)
    // Choose random a, b from prime field
    a = random(1, PRIME - 1)
    b = random(0, PRIME - 1)
    RETURN ((a * key + b) % PRIME) % tableSize
END FUNCTION
```

***

## 6. Collision Resolution Strategies

### a. Separate Chaining
Each bucket stores linked list of colliding entries.[8][4]

**Pros:**
- Simple implementation[8]
- Graceful degradation[8]
- Load factor > 1 possible[8]

**Cons:**
- Extra memory for pointers[8]
- Poor cache locality[8]

### b. Open Addressing

#### Linear Probing
```text
Probe sequence: h(key), h(key)+1, h(key)+2, ...
```
**Pros:** Excellent cache locality[8]
**Cons:** Primary clustering[8]

#### Quadratic Probing
```text
Probe sequence: h(key), h(key)+1², h(key)+4², h(key)+9², ...
```
**Pros:** Reduces clustering[8]
**Cons:** Secondary clustering; requires prime table size[8]

#### Double Hashing
```text
Probe sequence: h1(key), h1(key)+d·h2(key), h1(key)+2d·h2(key), ...
```
**Pros:** Best distribution; avoids clustering[8]
**Cons:** Two hash computations[8]

### c. Advanced Techniques

#### Cuckoo Hashing
Multiple hash functions; displace existing entries.[8]
- **Complexity:** O(1) worst-case lookup[8]

#### Robin Hood Hashing
Minimize variance in probe distances.[8]
- **Benefit:** More uniform access times[8]

#### Hopscotch Hashing
Limit search radius to neighborhood.[8]
- **Benefit:** Excellent cache locality[8]

***

## 7. Load Factor and Resizing

### a. Load Factor
```text
α = n / m
```
- $$ n $$ = number of entries
- $$ m $$ = table size
- Determines collision rate[4]

### b. When to Resize
```text
Separate Chaining: α > 0.75
Open Addressing: α > 0.5
```

### c. Resizing Algorithm
```text
FUNCTION resize()
    oldBuckets = buckets
    oldSize = size
    
    size = oldSize * 2  // Or nextPrime(oldSize * 2)
    buckets = ARRAY[size]
    count = 0
    
    // Rehash all entries
    FOR bucket IN oldBuckets DO
        IF bucket != null THEN
            FOR entry IN bucket DO
                put(entry.key, entry.value)
            END FOR
        END IF
    END FOR
END FUNCTION
```

**Complexity:**
- Resize cost: O(n)[4]
- Amortized per insertion: O(1)[4]

***

## 8. Complexity Analysis

### Set Operations
| Operation | Hash Set | Tree Set | Linked Hash Set |
|-----------|----------|----------|-----------------|
| add | O(1) avg | O(log n) | O(1) avg |
| remove | O(1) avg | O(log n) | O(1) avg |
| contains | O(1) avg | O(log n) | O(1) avg |
| min/max | O(n) | O(log n) | O(n) |
| range query | O(n) | O(log n + k) | O(n) |
| Space | O(n) | O(n) | O(n) + pointers |

### Map Operations
| Operation | Hash Map | Tree Map | Linked Hash Map |
|-----------|----------|----------|-----------------|
| put | O(1) avg | O(log n) | O(1) avg |
| get | O(1) avg | O(log n) | O(1) avg |
| remove | O(1) avg | O(log n) | O(1) avg |
| containsKey | O(1) avg | O(log n) | O(1) avg |
| containsValue | O(n) | O(n) | O(n) |
| firstKey/lastKey | O(n) | O(log n) | O(1) |
| range query | O(n) | O(log n + k) | O(n) |
| Space | O(n) | O(n) | O(n) + pointers |

---

## 9. Real-World Applications

### Sets
- **Duplicate detection**: Remove duplicates from datasets[2]
- **Membership testing**: Check user permissions, blacklists[2]
- **Graph algorithms**: Visited nodes tracking[9][2]
- **Cache replacement**: LRU/LFU cache implementations[2]
- **Database indexing**: Unique constraints[2]
- **Spell checkers**: Dictionary lookups[4]
- **Network security**: IP blacklists/whitelists

### Maps
- **Caching**: Web server response caches (URL -> content)[4]
- **Symbol tables**: Compiler variable/function mappings[4]
- **Frequency counting**: Word counts, histograms[4]
- **Database indexing**: Primary keys -> record addresses[4]
- **Configuration storage**: Key-value config files
- **Session management**: Session ID -> user data
- **Routing tables**: IP address -> next hop
- **DNS resolution**: Domain name -> IP address

---

## 10. Advanced Patterns & Algorithms

### a. Disjoint Set Union-Find
Specialized set structure for connectivity problems.[9][7]

**Operations:**
- **find(element)**: Determine which subset element belongs to
- **union(elemA, elemB)**: Merge subsets containing elemA and elemB

**Pseudocode:**
```text
CLASS DisjointSet
    parent = MAP()
    rank = MAP()
    
    FUNCTION makeSet(element)
        parent[element] = element
        rank[element] = 0
    END FUNCTION
    
    FUNCTION find(element)
        // Path compression
        IF parent[element] != element THEN
            parent[element] = find(parent[element])
        END IF
        RETURN parent[element]
    END FUNCTION
    
    FUNCTION union(elemA, elemB)
        rootA = find(elemA)
        rootB = find(elemB)
        
        IF rootA == rootB THEN
            RETURN  // Already in same set
        END IF
        
        // Union by rank
        IF rank[rootA] < rank[rootB] THEN
            parent[rootA] = rootB
        ELSE IF rank[rootA] > rank[rootB] THEN
            parent[rootB] = rootA
        ELSE
            parent[rootB] = rootA
            rank[rootA] += 1
        END IF
    END FUNCTION
    
    FUNCTION isConnected(elemA, elemB)
        RETURN find(elemA) == find(elemB)
    END FUNCTION
END CLASS
```

**Complexity:**
- **find/union**: O(α(n)) ≈ O(1) amortized (with path compression + union by rank)
- α(n) is inverse Ackermann function (effectively constant)

**Applications:**
- Kruskal's minimum spanning tree[9]
- Network connectivity[9]
- Image segmentation[9]
- Cycle detection in graphs[7]

### b. LRU Cache (Map + Doubly-Linked List)
Combines hash map and doubly-linked list for O(1) eviction.[1][4]

**Structure:**
```text
HashMap: key -> Node
Doubly-Linked List: MRU -> ... -> LRU
```

**Pseudocode:**
```text
CLASS LRUCache
    capacity = 0
    cache = HashMap()  // key -> Node
    head = Node(-1, -1)  // Dummy head (MRU)
    tail = Node(-1, -1)  // Dummy tail (LRU)
    
    FUNCTION __init__(capacity)
        THIS.capacity = capacity
        head.next = tail
        tail.prev = head
    END FUNCTION
    
    FUNCTION get(key)
        IF NOT cache.containsKey(key) THEN
            RETURN -1
        END IF
        
        node = cache[key]
        moveToFront(node)
        RETURN node.value
    END FUNCTION
    
    FUNCTION put(key, value)
        IF cache.containsKey(key) THEN
            node = cache[key]
            node.value = value
            moveToFront(node)
        ELSE
            IF cache.size() >= capacity THEN
                // Evict LRU
                lru = tail.prev
                removeNode(lru)
                cache.remove(lru.key)
            END IF
            
            node = Node(key, value)
            cache[key] = node
            addToFront(node)
        END IF
    END FUNCTION
    
    FUNCTION moveToFront(node)
        removeNode(node)
        addToFront(node)
    END FUNCTION
    
    FUNCTION removeNode(node)
        node.prev.next = node.next
        node.next.prev = node.prev
    END FUNCTION
    
    FUNCTION addToFront(node)
        node.next = head.next
        node.prev = head
        head.next.prev = node
        head.next = node
    END FUNCTION
END CLASS
```

**Complexity:**
- **get/put**: O(1) all operations[4]

**Applications:**
- Browser caches[4]
- CPU caches[4]
- Database buffer pools[4]

### c. Bloom Filter (Probabilistic Set)
Space-efficient probabilistic set; allows false positives.[1][4]

**Pseudocode:**
```text
CLASS BloomFilter
    bits = BITARRAY(size)
    size = 0
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
        RETURN true  // PROBABLY in set (false positive possible)
    END FUNCTION
END CLASS
```

**Complexity:**
- **add/contains**: O(k) where k = number of hash functions
- **Space**: O(m) bits (much smaller than hash set)

**Applications:**
- Spell checkers[4]
- URL duplicate detection[4]
- Cache filters[4]
- Network security (malicious URL detection)

### d. Consistent Hashing (Distributed Maps)
Minimizes remapping when nodes added/removed.[1][4]

**Pseudocode:**
```text
CLASS ConsistentHash
    ring = TreeMap()  // hash -> node
    numReplicas = 150
    
    FUNCTION addNode(node)
        FOR i = 0 TO numReplicas - 1 DO
            hash = hashFunction(node + "_" + i.toString())
            ring.put(hash, node)
        END FOR
    END FUNCTION
    
    FUNCTION removeNode(node)
        FOR i = 0 TO numReplicas - 1 DO
            hash = hashFunction(node + "_" + i.toString())
            ring.remove(hash)
        END FOR
    END FUNCTION
    
    FUNCTION getNode(key)
        hash = hashFunction(key)
        entry = ring.ceilingEntry(hash)  // Smallest hash >= key hash
        
        IF entry == null THEN
            entry = ring.firstEntry()  // Wrap around
        END IF
        
        RETURN entry.value
    END FUNCTION
END CLASS
```

**Complexity:**
- **getNode**: O(log n) where n = number of virtual nodes
- **addNode/removeNode**: O(r log n) where r = replicas

**Applications:**
- Distributed caches (Memcached, Redis Cluster)[8]
- Load balancing[4]
- Distributed hash tables (DHTs)[4]

***

## 11. FAANG & Top Company Interview Patterns

### Common Set Problems
- **Two Sum**: Use set for O(n) complement lookup
- **Contains Duplicate**: Add to set; check if already exists
- **Happy Number**: Track seen numbers; detect cycles
- **Intersection of Arrays**: Set for O(n) intersection
- **Longest Consecutive Sequence**: Set for O(n) sequence building
- **Valid Sudoku**: Set per row/column/box for duplicate detection

### Common Map Problems
- **Two Sum**: Map complement -> index
- **Group Anagrams**: Map sorted characters -> anagrams
- **LRU Cache**: Map + doubly-linked list
- **Top K Frequent Elements**: Map for frequency + heap
- **Design HashMap**: Implement from scratch (chaining/open addressing)
- **Subarray Sum Equals K**: Map prefix sums
- **Word Pattern**: Bidirectional mapping (BiMap)
- **Logger Rate Limiter**: Map timestamp tracking

### Company-Specific Focus
- **Google**: Hash table fundamentals, Bloom filters, consistent hashing[4]
- **Amazon**: LRU cache design, frequency counting, set operations[4]
- **Facebook**: Hash-based duplicate detection, multimap for grouping[4]
- **Microsoft**: Hash implementation, collision resolution[4]
- **Apple**: Hash function design, memory optimization[4]
- **Netflix**: Consistent hashing for CDN routing
- **Uber**: Geospatial indexing with maps

***

## 12. Common Pitfalls

### Sets
- **Not handling uniqueness**: Assuming list-like duplicate behavior
- **Order assumptions**: Hash sets are unordered
- **Modifying elements**: Changing element after insertion breaks hash
- **Inefficient contains checks**: O(n) list vs O(1) set

### Maps
- **Poor hash function**: Clustered collisions → O(n) operations[4]
- **Not resizing**: Load factor grows unbounded[4]
- **Mutable keys**: Changing key after insertion breaks map[4]
- **containsValue confusion**: O(n) operation, not O(1)
- **Null key/value handling**: Implementation-dependent (Java HashMap allows one null key)
- **Concurrent modification**: Modifying map while iterating

***

## 13. Best Practices

### General
- **Use built-in implementations**: HashMap, HashSet, TreeMap, TreeSet[4]
- **Immutable keys**: Prefer String, Integer over mutable objects[4]
- **Prime table sizes**: Reduces clustering (some implementations)[4]
- **Load factor 0.5-0.75**: Balances speed vs space[4]

### Performance Optimization
- **Choose right implementation**:
  - Hash-based: Fast O(1) operations, unordered
  - Tree-based: Sorted order, O(log n) operations
  - Linked: Predictable iteration order
- **Batch operations**: Minimize resizing overhead
- **Pre-size collections**: Set initial capacity if size known
- **Cache hash codes**: For custom objects with expensive hashing

### Security
- **Cryptographic hashing**: For sensitive data (passwords)[4]
- **Universal hashing**: Defeat adversarial inputs[4]
- **Rate limiting**: Map-based tracking for API throttling

***

## 14. Language-Specific Implementations

### Python
```python
# Set
s = set()  # HashSet
s.add(5)
s.remove(5)
5 in s  # contains

# Map
m = dict()  # HashMap
m['key'] = 'value'
del m['key']
'key' in m  # containsKey

from collections import OrderedDict, defaultdict, Counter
ordered = OrderedDict()  # LinkedHashMap
counter = Counter()  # Multiset (frequency map)
multi = defaultdict(list)  # Multimap
```

### Java
```java
// Set
Set<Integer> hashSet = new HashSet<>();
Set<Integer> treeSet = new TreeSet<>();
Set<Integer> linkedSet = new LinkedHashSet<>();

// Map
Map<String, Integer> hashMap = new HashMap<>();
Map<String, Integer> treeMap = new TreeMap<>();
Map<String, Integer> linkedMap = new LinkedHashMap<>();

// Multiset (via Guava)
Multiset<String> multiset = HashMultiset.create();

// Multimap (via Guava)
Multimap<String, Integer> multimap = ArrayListMultimap.create();
```

### C#
```csharp
// Set
HashSet<int> hashSet = new HashSet<int>();
SortedSet<int> sortedSet = new SortedSet<int>();

// Map
Dictionary<string, int> dict = new Dictionary<string, int>();
SortedDictionary<string, int> sortedDict = new SortedDictionary<string, int>();
```

### C++
```cpp
// Set
#include <unordered_set>  // HashSet
#include <set>  // TreeSet

std::unordered_set<int> hashSet;
std::set<int> treeSet;

// Map
#include <unordered_map>  // HashMap
#include <map>  // TreeMap

std::unordered_map<std::string, int> hashMap;
std::map<std::string, int> treeMap;

// Multiset
#include <unordered_multiset>
std::unordered_multiset<int> multiset;

// Multimap
#include <unordered_multimap>
std::unordered_multimap<std::string, int> multimap;
```

### JavaScript
```javascript
// Set
const set = new Set();
set.add(5);
set.has(5);
set.delete(5);

// Map
const map = new Map();
map.set('key', 'value');
map.get('key');
map.has('key');
map.delete('key');

// WeakSet (keys can be garbage collected)
const weakSet = new WeakSet();

// WeakMap (keys can be garbage collected)
const weakMap = new WeakMap();
```

***

## 15. Advanced Topics & Research

### Modern Techniques
- **Universal Hashing**: Randomized hash family; O(log n) worst-case[4]
- **Robin Hood Hashing**: Minimize variance in probe lengths[8][4]
- **Hopscotch Hashing**: Neighborhood-based probing[8][4]
- **Swiss Tables**: Google's flat hash map (C++ Abseil)
- **F14**: Facebook's hash table with SIMD optimizations

### Distributed Systems
- **Distributed Hash Tables (DHT)**: Chord, Kademlia, Pastry[4]
- **Rendezvous Hashing**: Highest random weight (HRW)
- **Jump Consistent Hash**: Google's lightweight consistent hashing
- **Maglev Hashing**: Google's load balancer hashing

### Cryptographic Applications
- **Merkle Trees**: Hash-based data structure for blockchain
- **Hash-based signatures**: Post-quantum cryptography
- **HMAC**: Hash-based message authentication
- **Password hashing**: bcrypt, Argon2, scrypt

### Probabilistic Data Structures
- **Bloom Filter**: Membership testing[4]
- **Count-Min Sketch**: Frequency estimation[4]
- **HyperLogLog**: Cardinality estimation
- **Cuckoo Filter**: Improved Bloom filter with deletions

***

## 16. Performance Benchmarks

### Operation Comparison (1M elements)

| Operation | Hash Set | Tree Set | Linked Hash Set |
|-----------|----------|----------|-----------------|
| Insert all | ~50ms | ~200ms | ~60ms |
| Contains (hit) | ~0.001ms | ~0.02ms | ~0.001ms |
| Contains (miss) | ~0.001ms | ~0.02ms | ~0.001ms |
| Remove | ~0.001ms | ~0.02ms | ~0.001ms |
| Iteration | ~20ms | ~25ms | ~20ms |
| Memory | ~30MB | ~50MB | ~40MB |

*Benchmarks are approximate and vary by implementation*

---

## 17. Debugging & Visualization Tools

### Visualization
- **Hash table visualizers**: [visualgo.net](https://visualgo.net), [USF CSE](https://www.cs.usfca.edu/~galles/visualization/)
- **Set operation diagrams**: Venn diagrams for union/intersection
- **Collision resolution animations**: Separate chaining, linear probing

### Debugging Techniques
```text
// Log hash distribution
FUNCTION analyzeHashDistribution()
    bucketSizes = [0] * size
    FOR bucket IN buckets DO
        bucketSizes[index] = len(bucket)
    END FOR
    
    PRINT "Min:", min(bucketSizes)
    PRINT "Max:", max(bucketSizes)
    PRINT "Avg:", sum(bucketSizes) / size
    PRINT "Std Dev:", standardDeviation(bucketSizes)
END FUNCTION

// Detect clustering
FUNCTION detectClustering()
    maxChain = 0
    FOR bucket IN buckets DO
        maxChain = max(maxChain, len(bucket))
    END FOR
    
    IF maxChain > 2 * (count / size) THEN
        PRINT "Warning: Clustering detected!"
    END IF
END FUNCTION
```

***

## 18. Summary & Learning Roadmap

### Mastery Progression

#### Level 1: Foundations (Weeks 1-2)
- Understand set/map concepts and operations
- Learn hash function basics
- Implement simple hash set/map with chaining
- Practice basic operations: add, remove, contains

#### Level 2: Collision Resolution (Weeks 3-4)
- Master separate chaining implementation
- Implement linear/quadratic probing
- Understand load factor and resizing
- Compare collision strategies

#### Level 3: Advanced Implementations (Weeks 5-6)
- Implement tree-based set/map
- Build linked hash set/map
- Create multiset and multimap
- Optimize hash functions

#### Level 4: Real-World Patterns (Weeks 7-8)
- Solve LRU cache problem
- Implement consistent hashing
- Build Bloom filter
- Practice FAANG interview problems

#### Level 5: Systems & Optimization (Weeks 9-10)
- Distributed hash tables
- Disjoint set union-find
- Performance profiling and tuning
- Production system design

### Key Takeaways
- **Sets**: Unique elements, fast membership testing, set operations
- **Maps**: Key-value pairs, O(1) average lookup, versatile applications
- **Hash-based**: Fast O(1) operations, unordered
- **Tree-based**: Sorted order, O(log n) operations, range queries
- **Trade-offs**: Speed vs order vs memory
- **Applications**: Caching, indexing, duplicate detection, frequency counting

### Essential Skills
✅ Implement hash set/map from scratch  
✅ Design effective hash functions  
✅ Choose appropriate collision resolution  
✅ Optimize load factor and resizing  
✅ Apply to interview problems (Two Sum, LRU Cache)  
✅ Recognize real-world use cases  
✅ Avoid common pitfalls (mutable keys, poor hashing)  

---

**Sets and maps are the workhorses of modern computing—powering search engines, databases, compilers, caches, and distributed systems. Master them to build lightning-fast applications and dominate technical interviews!**[3][2][4]

[1](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/87948039/74a4f14c-ae28-40ab-aba5-10e9b4548725/PSEUDOCODE.md)
[2](https://www.geeksforgeeks.org/dsa/introduction-to-set-data-structure/)
[3](https://www.geeksforgeeks.org/dsa/introduction-to-map-data-structure/)
[4](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/87948039/be8cb354-7753-435a-9511-201a41ae0036/DESCRIPTION.md)
[5](https://www.geeksforgeeks.org/java/hashset-in-java/)
[6](https://www.w3schools.com/dsa/dsa_data_hashsets.php)
[7](https://blog.logrocket.com/javascript-maps-vs-sets-choosing-your-data-structure/)
[8](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/87948039/e8d1ce7d-f58b-4d61-af51-99ebdc6fb2cc/COLLISION.md)
[9](https://blog.heycoach.in/exploring-advanced-algorithms-and-data-structures/)
[10](https://roadmap.sh/datastructures-and-algorithms)
[11](https://meritshot.com/blogs/data-structures-and-algorithms-roadmap-2025-what-top-developers-are-learning-right-now)
[12](https://www.usdsi.org/data-science-insights/understanding-data-structures-in-2025)
[13](https://www.cs.cmu.edu/~15850/notes/cmu850-f20.pdf)