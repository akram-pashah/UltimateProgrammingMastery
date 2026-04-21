# 🔥 Special Structures: Advanced Data Structures - Complete Deep Dive

This comprehensive guide covers specialized data structures that solve unique problems with exceptional efficiency: Union-Find, Bloom Filters, LRU/LFU Cache, Rope, Suffix Arrays/Trees, Bitsets, and Fenwick Trees.[1][2][3][4]

***

## 1. Union-Find (Disjoint Set Union)

### Core Concepts

Union-Find is a data structure that tracks elements partitioned into disjoint (non-overlapping) sets and supports two operations efficiently: finding which set an element belongs to, and merging two sets.[5][6]

**Key Operations:**
- **find(x)**: Determine which set x belongs to (returns representative/root)
- **union(x, y)**: Merge sets containing x and y
- **isConnected(x, y)**: Check if x and y are in same set

**Time Complexity:** O(α(n)) ≈ O(1) amortized with optimizations[6][5]

---

### Implementation with All Optimizations

```text
CLASS UnionFind
    parent = ARRAY[n]
    rank = ARRAY[n]
    size = ARRAY[n]
    componentCount = n
    
    FUNCTION init(n)
        FOR i = 0 TO n-1 DO
            parent[i] = i      // Each element is its own parent
            rank[i] = 0        // Initial rank is 0
            size[i] = 1        // Initial size is 1
        END FOR
        componentCount = n
    END FUNCTION
    
    // Find with Path Compression
    FUNCTION find(x)
        IF parent[x] != x THEN
            parent[x] = find(parent[x])  // Path compression
        END IF
        RETURN parent[x]
    END FUNCTION
    
    // Union by Rank
    FUNCTION unionByRank(x, y)
        rootX = find(x)
        rootY = find(y)
        
        IF rootX == rootY THEN
            RETURN false  // Already in same set
        END IF
        
        // Attach smaller rank tree under larger rank tree
        IF rank[rootX] < rank[rootY] THEN
            parent[rootX] = rootY
            size[rootY] += size[rootX]
        ELSE IF rank[rootX] > rank[rootY] THEN
            parent[rootY] = rootX
            size[rootX] += size[rootY]
        ELSE
            parent[rootY] = rootX
            rank[rootX] += 1
            size[rootX] += size[rootY]
        END IF
        
        componentCount -= 1
        RETURN true
    END FUNCTION
    
    // Union by Size (Alternative)
    FUNCTION unionBySize(x, y)
        rootX = find(x)
        rootY = find(y)
        
        IF rootX == rootY THEN
            RETURN false
        END IF
        
        // Attach smaller component to larger
        IF size[rootX] < size[rootY] THEN
            parent[rootX] = rootY
            size[rootY] += size[rootX]
        ELSE
            parent[rootY] = rootX
            size[rootX] += size[rootY]
        END IF
        
        componentCount -= 1
        RETURN true
    END FUNCTION
    
    FUNCTION isConnected(x, y)
        RETURN find(x) == find(y)
    END FUNCTION
    
    FUNCTION getComponentSize(x)
        RETURN size[find(x)]
    END FUNCTION
    
    FUNCTION getComponentCount()
        RETURN componentCount
    END FUNCTION
END CLASS
```

***

### Advanced Union-Find Variants

**1. Union-Find with Rollback (Persistent DSU):**
```text
CLASS PersistentUnionFind
    parent = ARRAY[n]
    rank = ARRAY[n]
    history = STACK()  // Store undo operations
    
    FUNCTION union(x, y)
        rootX = find(x)
        rootY = find(y)
        
        IF rootX == rootY THEN
            history.push(NO_CHANGE)
            RETURN false
        END IF
        
        IF rank[rootX] < rank[rootY] THEN
            parent[rootX] = rootY
            history.push((PARENT_CHANGE, rootX, rootX))
        ELSE IF rank[rootX] > rank[rootY] THEN
            parent[rootY] = rootX
            history.push((PARENT_CHANGE, rootY, rootY))
        ELSE
            parent[rootY] = rootX
            oldRank = rank[rootX]
            rank[rootX] += 1
            history.push((RANK_CHANGE, rootX, oldRank, rootY, rootY))
        END IF
        
        RETURN true
    END FUNCTION
    
    FUNCTION rollback()
        operation = history.pop()
        IF operation == NO_CHANGE THEN
            RETURN
        END IF
        
        IF operation.type == PARENT_CHANGE THEN
            parent[operation.node] = operation.oldParent
        ELSE IF operation.type == RANK_CHANGE THEN
            rank[operation.node] = operation.oldRank
            parent[operation.otherNode] = operation.oldParent
        END IF
    END FUNCTION
END CLASS

Use Case: Online/offline query processing, backtracking algorithms
```

**2. Weighted Union-Find:**
```text
CLASS WeightedUnionFind
    parent = ARRAY[n]
    weight = ARRAY[n]  // Distance to parent
    
    FUNCTION find(x)
        IF parent[x] != x THEN
            oldParent = parent[x]
            parent[x] = find(parent[x])
            weight[x] += weight[oldParent]  // Update weight
        END IF
        RETURN parent[x]
    END FUNCTION
    
    FUNCTION union(x, y, w)
        // Connect x to y with weight w
        rootX = find(x)
        rootY = find(y)
        
        IF rootX == rootY THEN
            RETURN weight[y] - weight[x] == w
        END IF
        
        parent[rootX] = rootY
        weight[rootX] = weight[y] - weight[x] + w
        RETURN true
    END FUNCTION
    
    FUNCTION getDistance(x, y)
        IF find(x) != find(y) THEN
            RETURN INFINITY  // Not connected
        END IF
        RETURN weight[x] - weight[y]
    END FUNCTION
END CLASS

Use Case: Equations with variables, distance calculations
```

***

## 2. Bloom Filter

### Core Concepts

A Bloom Filter is a space-efficient probabilistic data structure for testing set membership. It can have **false positives** but never false negatives.[7]

**Properties:**
- Space-efficient: ~1.2 bytes per element vs ~32 bytes for hash set
- Fast operations: O(k) where k = number of hash functions
- Trade-off: Small false positive rate for massive space savings
- Cannot remove elements (standard version)

---

### Complete Implementation

```text
CLASS BloomFilter
    bits = BITARRAY(m)
    m = bit array size
    k = number of hash functions
    n = 0  // Elements added
    
    FUNCTION init(expectedElements, falsePositiveRate)
        // Calculate optimal size
        m = CEILING(-expectedElements * LN(falsePositiveRate) / (LN(2)^2))
        
        // Calculate optimal hash function count
        k = CEILING((m / expectedElements) * LN(2))
        
        bits = BITARRAY(m) filled with 0
        n = 0
    END FUNCTION
    
    // Double hashing technique
    FUNCTION hash(element, i)
        h1 = murmurHash(element)
        h2 = fnvHash(element)
        RETURN (h1 + i * h2) MOD m
    END FUNCTION
    
    FUNCTION add(element)
        FOR i = 0 TO k-1 DO
            index = hash(element, i)
            bits[index] = 1
        END FOR
        n += 1
    END FUNCTION
    
    FUNCTION contains(element)
        FOR i = 0 TO k-1 DO
            index = hash(element, i)
            IF bits[index] == 0 THEN
                RETURN false  // Definitely NOT present
            END IF
        END FOR
        RETURN true  // Probably present
    END FUNCTION
    
    FUNCTION estimateFalsePositiveRate()
        // Formula: (1 - e^(-kn/m))^k
        exponent = (-k * n) / m
        RETURN (1 - EXP(exponent)) ^ k
    END FUNCTION
    
    FUNCTION estimateElementCount()
        // Count set bits
        X = countSetBits(bits)
        
        // Formula: n ≈ -(m/k) * ln(1 - X/m)
        RETURN -(m / k) * LN(1 - X / m)
    END FUNCTION
END CLASS
```

***

### Bloom Filter Variants

**1. Counting Bloom Filter (Supports Deletion):**
```text
CLASS CountingBloomFilter
    counters = ARRAY[m]  // 4-bit counters instead of 1-bit
    
    FUNCTION add(element)
        FOR i = 0 TO k-1 DO
            index = hash(element, i)
            IF counters[index] < 15 THEN  // Prevent overflow
                counters[index] += 1
            END IF
        END FOR
    END FUNCTION
    
    FUNCTION remove(element)
        IF NOT contains(element) THEN
            RETURN false  // May not actually be present
        END IF
        
        FOR i = 0 TO k-1 DO
            index = hash(element, i)
            IF counters[index] > 0 THEN
                counters[index] -= 1
            END IF
        END FOR
        RETURN true
    END FUNCTION
    
    FUNCTION contains(element)
        FOR i = 0 TO k-1 DO
            index = hash(element, i)
            IF counters[index] == 0 THEN
                RETURN false
            END IF
        END FOR
        RETURN true
    END FUNCTION
END CLASS

Space: 4× more than standard Bloom filter
Use Case: Cache systems with TTL, dynamic sets
```

**2. Scalable Bloom Filter:**
```text
CLASS ScalableBloomFilter
    filters = LIST of BloomFilters
    growthFactor = 2
    tighteningRatio = 0.5
    
    FUNCTION add(element)
        IF filters.isEmpty() OR filters.last().isFull() THEN
            newSize = filters.size() * growthFactor
            newFPR = filters.fpRate * tighteningRatio
            filters.append(BloomFilter(newSize, newFPR))
        END IF
        
        filters.last().add(element)
    END FUNCTION
    
    FUNCTION contains(element)
        FOR filter IN filters DO
            IF filter.contains(element) THEN
                RETURN true
            END IF
        END FOR
        RETURN false
    END FUNCTION
END CLASS

Use Case: Unbounded set size, streaming data
```

***

## 3. LRU Cache (Least Recently Used)

### Core Implementation

```text
CLASS Node
    key, value
    prev = null
    next = null
END CLASS

CLASS LRUCache
    capacity
    cache = HASHMAP()  // key → Node
    head = Node(-1, -1)  // Dummy head (MRU)
    tail = Node(-1, -1)  // Dummy tail (LRU)
    
    FUNCTION init(capacity)
        THIS.capacity = capacity
        head.next = tail
        tail.prev = head
    END FUNCTION
    
    FUNCTION get(key)
        IF key NOT IN cache THEN
            RETURN -1
        END IF
        
        node = cache[key]
        moveToHead(node)
        RETURN node.value
    END FUNCTION
    
    FUNCTION put(key, value)
        IF key IN cache THEN
            node = cache[key]
            node.value = value
            moveToHead(node)
        ELSE
            IF size(cache) >= capacity THEN
                lru = tail.prev
                removeNode(lru)
                DELETE cache[lru.key]
            END IF
            
            node = Node(key, value)
            cache[key] = node
            addToHead(node)
        END IF
    END FUNCTION
    
    FUNCTION moveToHead(node)
        removeNode(node)
        addToHead(node)
    END FUNCTION
    
    FUNCTION removeNode(node)
        node.prev.next = node.next
        node.next.prev = node.prev
    END FUNCTION
    
    FUNCTION addToHead(node)
        node.next = head.next
        node.prev = head
        head.next.prev = node
        head.next = node
    END FUNCTION
END CLASS
```

**Complexity:** O(1) for both get and put[8]

***

## 4. LFU Cache (Least Frequently Used)

### Complete Implementation

```text
CLASS LFUCache
    capacity
    minFreq = 0
    keyToValue = HASHMAP()  // key → value
    keyToFreq = HASHMAP()   // key → frequency
    freqToKeys = HASHMAP()  // frequency → OrderedSet of keys
    
    FUNCTION init(capacity)
        THIS.capacity = capacity
    END FUNCTION
    
    FUNCTION get(key)
        IF key NOT IN keyToValue THEN
            RETURN -1
        END IF
        
        updateFrequency(key)
        RETURN keyToValue[key]
    END FUNCTION
    
    FUNCTION put(key, value)
        IF capacity <= 0 THEN
            RETURN
        END IF
        
        IF key IN keyToValue THEN
            keyToValue[key] = value
            updateFrequency(key)
            RETURN
        END IF
        
        IF size(keyToValue) >= capacity THEN
            evictLFU()
        END IF
        
        keyToValue[key] = value
        keyToFreq[key] = 1
        minFreq = 1
        
        IF 1 NOT IN freqToKeys THEN
            freqToKeys[1] = OrderedSet()
        END IF
        freqToKeys[1].add(key)
    END FUNCTION
    
    FUNCTION updateFrequency(key)
        freq = keyToFreq[key]
        keyToFreq[key] = freq + 1
        
        freqToKeys[freq].remove(key)
        IF freqToKeys[freq].isEmpty() THEN
            DELETE freqToKeys[freq]
            IF minFreq == freq THEN
                minFreq += 1
            END IF
        END IF
        
        IF freq+1 NOT IN freqToKeys THEN
            freqToKeys[freq+1] = OrderedSet()
        END IF
        freqToKeys[freq+1].add(key)
    END FUNCTION
    
    FUNCTION evictLFU()
        evictKey = freqToKeys[minFreq].first()
        freqToKeys[minFreq].remove(evictKey)
        
        IF freqToKeys[minFreq].isEmpty() THEN
            DELETE freqToKeys[minFreq]
        END IF
        
        DELETE keyToValue[evictKey]
        DELETE keyToFreq[evictKey]
    END FUNCTION
END CLASS
```

**Complexity:** O(1) for all operations[8]

---

## 5. Rope Data Structure

### Core Concepts

A **Rope** is a binary tree for efficiently storing and manipulating very long strings. Instead of storing strings as contiguous arrays, ropes break them into smaller chunks stored in tree leaves.[9][10][1]

**Advantages:**
- Fast concatenation: O(1) or O(log n) vs O(n)
- Efficient insertion/deletion: O(log n) vs O(n)
- No large contiguous memory needed
- Easy undo operations (keep old tree roots)

**Disadvantages:**
- Slower character access: O(log n) vs O(1)
- More complex implementation
- Extra memory for tree nodes

***

### Implementation

```text
CLASS RopeNode
    weight = 0       // Characters in left subtree
    left = null
    right = null
    data = ""        // Only for leaf nodes
    
    FUNCTION isLeaf()
        RETURN left == null AND right == null
    END FUNCTION
END CLASS

CLASS Rope
    root = null
    maxLeafLength = 8  // Configurable chunk size
    
    FUNCTION init(string)
        root = buildRope(string, 0, length(string))
    END FUNCTION
    
    FUNCTION buildRope(str, start, end)
        length = end - start
        
        IF length <= maxLeafLength THEN
            // Create leaf node
            node = RopeNode()
            node.data = substring(str, start, end)
            node.weight = length
            RETURN node
        END IF
        
        // Split and create internal node
        mid = start + length / 2
        node = RopeNode()
        node.left = buildRope(str, start, mid)
        node.right = buildRope(str, mid, end)
        node.weight = mid - start
        RETURN node
    END FUNCTION
    
    // Concatenate two ropes
    FUNCTION concat(other)
        newRoot = RopeNode()
        newRoot.left = THIS.root
        newRoot.right = other.root
        newRoot.weight = getWeight(THIS.root)
        RETURN Rope(newRoot)
    END FUNCTION
    
    // Access character at index
    FUNCTION charAt(index)
        RETURN charAtHelper(root, index)
    END FUNCTION
    
    FUNCTION charAtHelper(node, index)
        IF node.isLeaf() THEN
            RETURN node.data[index]
        END IF
        
        IF index < node.weight THEN
            RETURN charAtHelper(node.left, index)
        ELSE
            RETURN charAtHelper(node.right, index - node.weight)
        END IF
    END FUNCTION
    
    // Split rope at index
    FUNCTION split(index)
        leftRope = splitHelper(root, index, true)
        rightRope = splitHelper(root, index, false)
        RETURN (leftRope, rightRope)
    END FUNCTION
    
    FUNCTION splitHelper(node, index, takeLeft)
        IF node.isLeaf() THEN
            IF takeLeft THEN
                RETURN substring(node.data, 0, index)
            ELSE
                RETURN substring(node.data, index, length(node.data))
            END IF
        END IF
        
        IF index < node.weight THEN
            IF takeLeft THEN
                newNode = RopeNode()
                newNode.left = splitHelper(node.left, index, true)
                newNode.weight = index
                RETURN newNode
            ELSE
                newNode = RopeNode()
                newNode.left = splitHelper(node.left, index, false)
                newNode.right = node.right
                newNode.weight = node.weight - index
                RETURN newNode
            END IF
        ELSE
            IF takeLeft THEN
                newNode = RopeNode()
                newNode.left = node.left
                newNode.right = splitHelper(node.right, index - node.weight, true)
                newNode.weight = node.weight
                RETURN newNode
            ELSE
                RETURN splitHelper(node.right, index - node.weight, false)
            END IF
        END IF
    END FUNCTION
    
    // Insert string at index
    FUNCTION insert(index, str)
        (left, right) = split(index)
        middle = Rope(str)
        RETURN left.concat(middle).concat(right)
    END FUNCTION
    
    // Delete substring [start, end)
    FUNCTION delete(start, end)
        (left, temp) = split(start)
        (_, right) = split(end - start)
        RETURN left.concat(right)
    END FUNCTION
    
    // Convert to string
    FUNCTION toString()
        result = ""
        inorderTraversal(root, result)
        RETURN result
    END FUNCTION
    
    FUNCTION inorderTraversal(node, result)
        IF node == null THEN
            RETURN
        END IF
        
        IF node.isLeaf() THEN
            result += node.data
            RETURN
        END IF
        
        inorderTraversal(node.left, result)
        inorderTraversal(node.right, result)
    END FUNCTION
END CLASS
```

**Complexity:**
- Concatenation: O(1)
- Split: O(log n)
- Insert/Delete: O(log n)
- Access character: O(log n)
- String conversion: O(n)

**Use Cases:**
- Text editors (Emacs, Sublime Text)
- Undo/redo systems
- Large document manipulation
- Functional programming languages

[10][1][9]

***

## 6. Suffix Array & Suffix Tree

### Suffix Array

A **suffix array** is a sorted array of all suffixes of a string.[2][4]

```text
CLASS SuffixArray
    sa = ARRAY[n]  // Suffix array
    lcp = ARRAY[n] // Longest common prefix array
    
    // O(n log²n) construction
    FUNCTION buildNaive(text)
        n = length(text)
        suffixes = []
        
        FOR i = 0 TO n-1 DO
            suffixes.append((substring(text, i), i))
        END FOR
        
        SORT(suffixes)
        
        FOR i = 0 TO n-1 DO
            sa[i] = suffixes[i].index
        END FOR
    END FUNCTION
    
    // O(n log n) optimized construction
    FUNCTION buildOptimized(text)
        n = length(text)
        sa = ARRAY[n]
        rank = ARRAY[n]
        tempRank = ARRAY[n]
        
        // Initialize rank (first character)
        FOR i = 0 TO n-1 DO
            rank[i] = ASCII(text[i])
        END FOR
        
        k = 1
        WHILE k < n DO
            // Sort based on rank pairs
            sortBySuffixPairs(sa, rank, k)
            
            // Update ranks
            tempRank[sa[0]] = 0
            FOR i = 1 TO n-1 DO
                prev = sa[i-1]
                curr = sa[i]
                
                IF rank[prev] == rank[curr] AND 
                   rank[prev + k] == rank[curr + k] THEN
                    tempRank[curr] = tempRank[prev]
                ELSE
                    tempRank[curr] = tempRank[prev] + 1
                END IF
            END FOR
            
            rank = tempRank
            k *= 2
        END FOR
        
        RETURN sa
    END FUNCTION
    
    // Build LCP array (Kasai's algorithm)
    FUNCTION buildLCP(text, sa)
        n = length(text)
        lcp = ARRAY[n]
        rank = ARRAY[n]
        
        FOR i = 0 TO n-1 DO
            rank[sa[i]] = i
        END FOR
        
        h = 0
        FOR i = 0 TO n-1 DO
            IF rank[i] > 0 THEN
                j = sa[rank[i] - 1]
                
                WHILE i + h < n AND j + h < n AND 
                      text[i + h] == text[j + h] DO
                    h += 1
                END WHILE
                
                lcp[rank[i]] = h
                
                IF h > 0 THEN
                    h -= 1
                END IF
            END IF
        END FOR
        
        RETURN lcp
    END FUNCTION
    
    // Pattern matching in O(m log n)
    FUNCTION search(pattern)
        m = length(pattern)
        left = 0
        right = n - 1
        
        // Binary search for first occurrence
        WHILE left <= right DO
            mid = (left + right) / 2
            suffix = substring(text, sa[mid])
            
            IF suffix starts with pattern THEN
                RETURN sa[mid]  // Found
            ELSE IF pattern < suffix THEN
                right = mid - 1
            ELSE
                left = mid + 1
            END IF
        END FOR
        
        RETURN -1  // Not found
    END FUNCTION
END CLASS
```

**Applications:**
- Pattern matching: O(m log n)
- Longest common substring
- Data compression (BWT)
- Bioinformatics (DNA analysis)

[4][2]

***

### Suffix Tree

A **suffix tree** is a compressed trie of all suffixes.[2]

```text
CLASS SuffixTreeNode
    children = HASHMAP()  // char → node
    start = -1
    end = -1
    suffixLink = null
    suffixIndex = -1
END CLASS

CLASS SuffixTree
    root = SuffixTreeNode()
    text = ""
    
    // Ukkonen's algorithm: O(n) construction
    FUNCTION build(text)
        THIS.text = text + "$"  // Add sentinel
        n = length(THIS.text)
        
        // Build incrementally
        FOR i = 0 TO n-1 DO
            extendSuffixTree(i)
        END FOR
    END FUNCTION
    
    // Search pattern in O(m)
    FUNCTION search(pattern)
        node = root
        
        FOR char IN pattern DO
            IF char NOT IN node.children THEN
                RETURN false
            END IF
            node = node.children[char]
        END FOR
        
        RETURN true
    END FUNCTION
    
    // Find longest repeated substring
    FUNCTION longestRepeatedSubstring()
        maxLen = 0
        maxNode = null
        
        FUNCTION dfs(node, depth)
            IF node.suffixIndex == -1 THEN  // Internal node
                IF depth > maxLen THEN
                    maxLen = depth
                    maxNode = node
                END IF
                
                FOR child IN node.children.values() DO
                    dfs(child, depth + edgeLength(child))
                END FOR
            END IF
        END FUNCTION
        
        dfs(root, 0)
        RETURN extractString(maxNode, maxLen)
    END FUNCTION
END CLASS
```

**Applications:**
- Pattern matching: O(m)
- Longest repeated substring: O(n)
- Longest common substring: O(n+m)
- String comparisons

[2]

---

## 7. Bitset

### Core Concepts

A **bitset** is a fixed-size sequence of bits with efficient bitwise operations.[3][11]

```text
CLASS Bitset
    bits = ARRAY of integers  // Each int stores 32/64 bits
    size = n
    
    FUNCTION init(n)
        THIS.size = n
        numInts = CEILING(n / 64)
        bits = ARRAY[numInts] filled with 0
    END FUNCTION
    
    FUNCTION set(pos)
        blockIndex = pos / 64
        bitIndex = pos MOD 64
        bits[blockIndex] |= (1 << bitIndex)
    END FUNCTION
    
    FUNCTION reset(pos)
        blockIndex = pos / 64
        bitIndex = pos MOD 64
        bits[blockIndex] &= ~(1 << bitIndex)
    END FUNCTION
    
    FUNCTION flip(pos)
        blockIndex = pos / 64
        bitIndex = pos MOD 64
        bits[blockIndex] ^= (1 << bitIndex)
    END FUNCTION
    
    FUNCTION test(pos)
        blockIndex = pos / 64
        bitIndex = pos MOD 64
        RETURN (bits[blockIndex] & (1 << bitIndex)) != 0
    END FUNCTION
    
    // Set operations
    FUNCTION union(other)
        result = Bitset(size)
        FOR i = 0 TO length(bits)-1 DO
            result.bits[i] = bits[i] | other.bits[i]
        END FOR
        RETURN result
    END FUNCTION
    
    FUNCTION intersection(other)
        result = Bitset(size)
        FOR i = 0 TO length(bits)-1 DO
            result.bits[i] = bits[i] & other.bits[i]
        END FOR
        RETURN result
    END FUNCTION
    
    FUNCTION difference(other)
        result = Bitset(size)
        FOR i = 0 TO length(bits)-1 DO
            result.bits[i] = bits[i] & ~other.bits[i]
        END FOR
        RETURN result
    END FUNCTION
    
    FUNCTION count()
        total = 0
        FOR block IN bits DO
            total += popcount(block)  // Count set bits
        END FOR
        RETURN total
    END FUNCTION
    
    FUNCTION any()
        FOR block IN bits DO
            IF block != 0 THEN
                RETURN true
            END IF
        END FOR
        RETURN false
    END FUNCTION
    
    FUNCTION all()
        fullBlocks = size / 64
        FOR i = 0 TO fullBlocks-1 DO
            IF bits[i] != -1 THEN  // All bits set
                RETURN false
            END IF
        END FOR
        
        // Check remaining bits
        remaining = size MOD 64
        IF remaining > 0 THEN
            mask = (1 << remaining) - 1
            RETURN bits[fullBlocks] == mask
        END IF
        
        RETURN true
    END FUNCTION
END CLASS
```

**Complexity:** O(n/64) for most operations[11][3]

**Use Cases:**
- Set operations on integers
- Graph algorithms (visited arrays)
- Dynamic programming (state compression)
- Sieve of Eratosthenes

[3][11]

***

## 8. Fenwick Tree (Binary Indexed Tree)

### Complete Implementation

```text
CLASS FenwickTree
    BIT = ARRAY[n+1]
    n = size
    
    FUNCTION init(n)
        THIS.n = n
        BIT = ARRAY[n+1] filled with 0
    END FUNCTION
    
    FUNCTION update(index, delta)
        index += 1  // Convert to 1-indexed
        WHILE index <= n DO
            BIT[index] += delta
            index += (index & -index)
        END WHILE
    END FUNCTION
    
    FUNCTION prefixSum(index)
        index += 1  // Convert to 1-indexed
        sum = 0
        WHILE index > 0 DO
            sum += BIT[index]
            index -= (index & -index)
        END WHILE
        RETURN sum
    END FUNCTION
    
    FUNCTION rangeSum(left, right)
        IF left > 0 THEN
            RETURN prefixSum(right) - prefixSum(left - 1)
        ELSE
            RETURN prefixSum(right)
        END IF
    END FUNCTION
    
    FUNCTION buildOptimized(arr)
        n = length(arr)
        BIT = ARRAY[n+1]
        
        FOR i = 1 TO n DO
            BIT[i] = arr[i-1]
        END FOR
        
        FOR i = 1 TO n DO
            parent = i + (i & -i)
            IF parent <= n THEN
                BIT[parent] += BIT[i]
            END IF
        END FOR
    END FUNCTION
END CLASS
```

***

## Comparison Table

| Structure | Primary Use | Time | Space | Removable | Probabilistic |
|-----------|-------------|------|-------|-----------|---------------|
| Union-Find | Connectivity | O(α(n)) | O(n) | No | No |
| Bloom Filter | Membership | O(k) | O(m) bits | Variants | Yes |
| LRU Cache | Eviction | O(1) | O(n) | Yes | No |
| LFU Cache | Eviction | O(1) | O(n) | Yes | No |
| Rope | Strings | O(log n) | O(n) | Yes | No |
| Suffix Array | Pattern Match | O(log n) | O(n) | No | No |
| Suffix Tree | Pattern Match | O(m) | O(n) | No | No |
| Bitset | Set Ops | O(n/64) | O(n) bits | Yes | No |
| Fenwick Tree | Range Query | O(log n) | O(n) | No | No |

[1][4][5][7][3][8][2]

***

**These specialized structures are the power tools of advanced algorithms—master them to solve complex problems that seem impossible with basic data structures!**[4][1][3][2]

[1](https://www.geeksforgeeks.org/dsa/ropes-data-structure-fast-string-concatenation/)
[2](https://en.wikipedia.org/wiki/Suffix_array)
[3](https://www.geeksforgeeks.org/cpp/bitset-operator-in-c-stl/)
[4](https://cp-algorithms.com/string/suffix-array.html)
[5](https://www.geeksforgeeks.org/dsa/introduction-to-disjoint-set-data-structure-or-union-find-algorithm/)
[6](https://www.geeksforgeeks.org/dsa/union-by-rank-and-path-compression-in-union-find-algorithm/)
[7](https://www.upgrad.com/blog/bloom-filters-in-data-structure/)
[8](https://www.geeksforgeeks.org/system-design/lru-cache-implementation/)
[9](https://compile7.org/implement-data-structures/how-to-implement-rope-in-eiffel/)
[10](https://www.cs.tufts.edu/comp/150FP/archive/hans-boehm/ropes.pdf)
[11](https://codeforces.com/blog/entry/56214)
[12](https://heycoach.in/blog/rope-data-structure/)
[13](https://stackoverflow.com/questions/1863440/is-there-any-scenario-where-the-rope-data-structure-is-more-efficient-than-a-str)
[14](https://ethz.ch/content/dam/ethz/special-interest/infk/chair-program-method/pm/documents/Verify%20This/Challenges2024/verifyThis2024-Challenge-0.pdf)