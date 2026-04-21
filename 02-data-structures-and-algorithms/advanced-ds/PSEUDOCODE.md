# 🔥 Pseudocode: Advanced Data Structures - Complete Implementation Guide

This comprehensive guide provides detailed pseudocode implementations for all major advanced data structures, from basic operations to complex optimizations.[1][2][3][4]

***

## 1. Segment Tree Pseudocode

### Basic Segment Tree (Range Sum)

**Structure:**
```text
CLASS SegmentTree
    tree = ARRAY[4 * n]
    n = size of input array
    
    FUNCTION init(size)
        n = size
        tree = ARRAY[4 * n] filled with 0
    END FUNCTION
END CLASS
```

**Build Tree:**
```text
FUNCTION build(arr, node, left, right)
    // Base case: leaf node
    IF left == right THEN
        tree[node] = arr[left]
        RETURN
    END IF
    
    // Recursive case: internal node
    mid = (left + right) / 2
    
    // Build left and right subtrees
    build(arr, 2*node, left, mid)
    build(arr, 2*node+1, mid+1, right)
    
    // Merge: sum of children
    tree[node] = tree[2*node] + tree[2*node+1]
END FUNCTION

// Initial call: build(arr, 1, 0, n-1)
```

**Complexity:** O(n)[5][1]

***

**Range Query:**
```text
FUNCTION query(node, treeLeft, treeRight, queryLeft, queryRight)
    // No overlap
    IF queryRight < treeLeft OR queryLeft > treeRight THEN
        RETURN 0  // Identity element for sum
    END IF
    
    // Complete overlap
    IF queryLeft <= treeLeft AND treeRight <= queryRight THEN
        RETURN tree[node]
    END IF
    
    // Partial overlap - split query
    mid = (treeLeft + treeRight) / 2
    
    leftSum = query(2*node, treeLeft, mid, queryLeft, queryRight)
    rightSum = query(2*node+1, mid+1, treeRight, queryLeft, queryRight)
    
    RETURN leftSum + rightSum
END FUNCTION

// Initial call: query(1, 0, n-1, L, R)
```

**Complexity:** O(log n)[1][5]

***

**Point Update:**
```text
FUNCTION update(node, treeLeft, treeRight, index, newValue)
    // Reached the leaf node
    IF treeLeft == treeRight THEN
        tree[node] = newValue
        RETURN
    END IF
    
    mid = (treeLeft + treeRight) / 2
    
    // Determine which child contains the index
    IF index <= mid THEN
        update(2*node, treeLeft, mid, index, newValue)
    ELSE
        update(2*node+1, mid+1, treeRight, index, newValue)
    END IF
    
    // Recalculate current node after update
    tree[node] = tree[2*node] + tree[2*node+1]
END FUNCTION

// Initial call: update(1, 0, n-1, index, value)
```

**Complexity:** O(log n)[5][1]

---

### Segment Tree with Lazy Propagation

**Structure:**
```text
CLASS LazySegmentTree
    tree = ARRAY[4 * n]
    lazy = ARRAY[4 * n]  // Pending updates
    n = size of input array
    
    FUNCTION init(size)
        n = size
        tree = ARRAY[4 * n] filled with 0
        lazy = ARRAY[4 * n] filled with 0
    END FUNCTION
END CLASS
```

**Push Down Lazy Updates:**
```text
FUNCTION pushDown(node, left, right)
    // If there's a pending update
    IF lazy[node] != 0 THEN
        // Apply pending update to current node
        tree[node] += (right - left + 1) * lazy[node]
        
        // If not a leaf, propagate to children
        IF left != right THEN
            lazy[2*node] += lazy[node]
            lazy[2*node+1] += lazy[node]
        END IF
        
        // Clear current node's lazy value
        lazy[node] = 0
    END IF
END FUNCTION
```

**Range Update:**
```text
FUNCTION rangeUpdate(node, treeLeft, treeRight, queryLeft, queryRight, delta)
    // Apply pending updates first
    pushDown(node, treeLeft, treeRight)
    
    // No overlap
    IF queryRight < treeLeft OR queryLeft > treeRight THEN
        RETURN
    END IF
    
    // Complete overlap
    IF queryLeft <= treeLeft AND treeRight <= queryRight THEN
        // Mark lazy update
        lazy[node] += delta
        pushDown(node, treeLeft, treeRight)
        RETURN
    END IF
    
    // Partial overlap
    mid = (treeLeft + treeRight) / 2
    rangeUpdate(2*node, treeLeft, mid, queryLeft, queryRight, delta)
    rangeUpdate(2*node+1, mid+1, treeRight, queryLeft, queryRight, delta)
    
    // Recalculate after children updated
    pushDown(2*node, treeLeft, mid)
    pushDown(2*node+1, mid+1, treeRight)
    tree[node] = tree[2*node] + tree[2*node+1]
END FUNCTION
```

**Complexity:** O(log n) per update[6][7][1]

***

**Range Query with Lazy:**
```text
FUNCTION rangeQuery(node, treeLeft, treeRight, queryLeft, queryRight)
    // Apply pending updates
    pushDown(node, treeLeft, treeRight)
    
    // No overlap
    IF queryRight < treeLeft OR queryLeft > treeRight THEN
        RETURN 0
    END IF
    
    // Complete overlap
    IF queryLeft <= treeLeft AND treeRight <= queryRight THEN
        RETURN tree[node]
    END IF
    
    // Partial overlap
    mid = (treeLeft + treeRight) / 2
    leftSum = rangeQuery(2*node, treeLeft, mid, queryLeft, queryRight)
    rightSum = rangeQuery(2*node+1, mid+1, treeRight, queryLeft, queryRight)
    
    RETURN leftSum + rightSum
END FUNCTION
```

**Complexity:** O(log n)[7][1]

***

### Segment Tree Variants

**Range Minimum Query:**
```text
FUNCTION buildMin(arr, node, left, right)
    IF left == right THEN
        tree[node] = arr[left]
        RETURN
    END IF
    
    mid = (left + right) / 2
    buildMin(arr, 2*node, left, mid)
    buildMin(arr, 2*node+1, mid+1, right)
    
    // Merge: minimum of children
    tree[node] = MIN(tree[2*node], tree[2*node+1])
END FUNCTION

FUNCTION queryMin(node, treeLeft, treeRight, queryLeft, queryRight)
    IF queryRight < treeLeft OR queryLeft > treeRight THEN
        RETURN INFINITY  // Identity for minimum
    END IF
    
    IF queryLeft <= treeLeft AND treeRight <= queryRight THEN
        RETURN tree[node]
    END IF
    
    mid = (treeLeft + treeRight) / 2
    leftMin = queryMin(2*node, treeLeft, mid, queryLeft, queryRight)
    rightMin = queryMin(2*node+1, mid+1, treeRight, queryLeft, queryRight)
    
    RETURN MIN(leftMin, rightMin)
END FUNCTION
```

**Range GCD:**
```text
FUNCTION buildGCD(arr, node, left, right)
    IF left == right THEN
        tree[node] = arr[left]
        RETURN
    END IF
    
    mid = (left + right) / 2
    buildGCD(arr, 2*node, left, mid)
    buildGCD(arr, 2*node+1, mid+1, right)
    
    // Merge: GCD of children
    tree[node] = GCD(tree[2*node], tree[2*node+1])
END FUNCTION
```

***

## 2. Fenwick Tree (Binary Indexed Tree) Pseudocode

### Basic Fenwick Tree

**Structure:**
```text
CLASS FenwickTree
    BIT = ARRAY[n+1]  // 1-indexed
    n = size of input array
    
    FUNCTION init(size)
        n = size
        BIT = ARRAY[n+1] filled with 0
    END FUNCTION
END CLASS
```

**Update Operation:**
```text
FUNCTION update(index, delta)
    // Convert to 1-indexed
    index = index + 1
    
    // Update all affected positions
    WHILE index <= n DO
        BIT[index] += delta
        
        // Move to next affected index
        index += (index & -index)  // Add last set bit
    END WHILE
END FUNCTION
```

**Explanation:** `index & -index` isolates the least significant set bit[3][4]

**Complexity:** O(log n)[4][3]

***

**Prefix Sum Query:**
```text
FUNCTION prefixSum(index)
    // Convert to 1-indexed
    index = index + 1
    sum = 0
    
    // Traverse ancestors
    WHILE index > 0 DO
        sum += BIT[index]
        
        // Move to parent index
        index -= (index & -index)  // Remove last set bit
    END WHILE
    
    RETURN sum
END FUNCTION
```

**Complexity:** O(log n)[3][4]

***

**Range Sum Query:**
```text
FUNCTION rangeSum(left, right)
    IF left > 0 THEN
        RETURN prefixSum(right) - prefixSum(left - 1)
    ELSE
        RETURN prefixSum(right)
    END IF
END FUNCTION
```

**Complexity:** O(log n)[4][3]

***

**Build Fenwick Tree:**
```text
FUNCTION build(arr)
    n = length(arr)
    BIT = ARRAY[n+1] filled with 0
    
    FOR i = 0 TO n-1 DO
        update(i, arr[i])
    END FOR
END FUNCTION
```

**Complexity:** O(n log n)[3][4]

**Optimized Build (O(n)):**
```text
FUNCTION buildOptimized(arr)
    n = length(arr)
    BIT = ARRAY[n+1] filled with 0
    
    // Copy array to BIT (1-indexed)
    FOR i = 1 TO n DO
        BIT[i] = arr[i-1]
    END FOR
    
    // Build tree bottom-up
    FOR i = 1 TO n DO
        parent = i + (i & -i)
        IF parent <= n THEN
            BIT[parent] += BIT[i]
        END IF
    END FOR
END FUNCTION
```

**Complexity:** O(n)[4][3]

***

### 2D Fenwick Tree

**Structure:**
```text
CLASS FenwickTree2D
    BIT = 2D_ARRAY[rows+1][cols+1]
    rows, cols
    
    FUNCTION init(r, c)
        rows = r
        cols = c
        BIT = 2D_ARRAY[rows+1][cols+1] filled with 0
    END FUNCTION
END CLASS
```

**Update:**
```text
FUNCTION update2D(row, col, delta)
    row = row + 1
    col = col + 1
    
    i = row
    WHILE i <= rows DO
        j = col
        WHILE j <= cols DO
            BIT[i][j] += delta
            j += (j & -j)
        END WHILE
        i += (i & -i)
    END WHILE
END FUNCTION
```

**Complexity:** O(log m × log n)[4]

***

**Prefix Sum 2D:**
```text
FUNCTION prefixSum2D(row, col)
    row = row + 1
    col = col + 1
    sum = 0
    
    i = row
    WHILE i > 0 DO
        j = col
        WHILE j > 0 DO
            sum += BIT[i][j]
            j -= (j & -j)
        END WHILE
        i -= (i & -i)
    END WHILE
    
    RETURN sum
END FUNCTION
```

**Range Sum 2D:**
```text
FUNCTION rangeSum2D(row1, col1, row2, col2)
    // Use inclusion-exclusion principle
    RETURN prefixSum2D(row2, col2)
         - prefixSum2D(row1-1, col2)
         - prefixSum2D(row2, col1-1)
         + prefixSum2D(row1-1, col1-1)
END FUNCTION
```

**Complexity:** O(log m × log n)[4]

***

## 3. Union-Find (Disjoint Set Union) Pseudocode

### Basic Union-Find with Optimizations

**Structure:**
```text
CLASS UnionFind
    parent = ARRAY[n]
    rank = ARRAY[n]
    count = n  // Number of components
    
    FUNCTION init(size)
        FOR i = 0 TO size-1 DO
            parent[i] = i
            rank[i] = 0
        END FOR
        count = size
    END FUNCTION
END CLASS
```

**Find with Path Compression:**
```text
FUNCTION find(x)
    // Path compression: make every node point to root
    IF parent[x] != x THEN
        parent[x] = find(parent[x])
    END IF
    RETURN parent[x]
END FUNCTION
```

**Explanation:** Path compression flattens the tree structure[2][8]

**Complexity:** O(α(n)) amortized, where α is inverse Ackermann[8][2]

---

**Iterative Find (Alternative):**
```text
FUNCTION findIterative(x)
    root = x
    
    // Find root
    WHILE parent[root] != root DO
        root = parent[root]
    END WHILE
    
    // Path compression
    WHILE parent[x] != root DO
        next = parent[x]
        parent[x] = root
        x = next
    END WHILE
    
    RETURN root
END FUNCTION
```

***

**Union by Rank:**
```text
FUNCTION union(x, y)
    rootX = find(x)
    rootY = find(y)
    
    // Already in same set
    IF rootX == rootY THEN
        RETURN false
    END IF
    
    // Union by rank: attach smaller tree under larger
    IF rank[rootX] < rank[rootY] THEN
        parent[rootX] = rootY
    ELSE IF rank[rootX] > rank[rootY] THEN
        parent[rootY] = rootX
    ELSE
        // Equal rank: choose arbitrary root and increment rank
        parent[rootY] = rootX
        rank[rootX] += 1
    END IF
    
    count -= 1  // Decrease number of components
    RETURN true
END FUNCTION
```

**Complexity:** O(α(n)) amortized[2][8]

***

**Union by Size (Alternative):**
```text
CLASS UnionFindBySize
    parent = ARRAY[n]
    size = ARRAY[n]
    
    FUNCTION init(n)
        FOR i = 0 TO n-1 DO
            parent[i] = i
            size[i] = 1
        END FOR
    END FUNCTION
    
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
        
        RETURN true
    END FUNCTION
    
    FUNCTION getSize(x)
        RETURN size[find(x)]
    END FUNCTION
END CLASS
```

***

**Additional Operations:**
```text
FUNCTION isConnected(x, y)
    RETURN find(x) == find(y)
END FUNCTION

FUNCTION getComponentCount()
    RETURN count
END FUNCTION

FUNCTION getComponentSize(x)
    root = find(x)
    componentSize = 0
    FOR i = 0 TO n-1 DO
        IF find(i) == root THEN
            componentSize += 1
        END IF
    END FOR
    RETURN componentSize
END FUNCTION
```

***

## 4. Bloom Filter Pseudocode

### Basic Bloom Filter

**Structure:**
```text
CLASS BloomFilter
    bits = BITARRAY(m)  // m bits
    k = number of hash functions
    hashFunctions = ARRAY[k]
    m = bit array size
    n = 0  // Number of elements added
    
    FUNCTION init(expectedElements, falsePositiveRate)
        // Calculate optimal parameters
        m = CEILING(-expectedElements * LN(falsePositiveRate) / (LN(2)^2))
        k = CEILING((m / expectedElements) * LN(2))
        bits = BITARRAY(m) filled with 0
    END FUNCTION
END CLASS
```

**Hash Functions:**
```text
FUNCTION hash1(element)
    // Use standard hash (e.g., MurmurHash)
    RETURN murmurHash(element) MOD m
END FUNCTION

FUNCTION hash2(element)
    // Use different hash
    RETURN fnvHash(element) MOD m
END FUNCTION

FUNCTION getHashValue(element, i)
    // Double hashing technique
    h1 = hash1(element)
    h2 = hash2(element)
    RETURN (h1 + i * h2) MOD m
END FUNCTION
```

***

**Add Element:**
```text
FUNCTION add(element)
    FOR i = 0 TO k-1 DO
        index = getHashValue(element, i)
        bits[index] = 1
    END FOR
    n += 1
END FUNCTION
```

**Complexity:** O(k)[9]

***

**Contains Check:**
```text
FUNCTION contains(element)
    FOR i = 0 TO k-1 DO
        index = getHashValue(element, i)
        IF bits[index] == 0 THEN
            RETURN false  // Definitely NOT present
        END IF
    END FOR
    RETURN true  // Probably present (may be false positive)
END FUNCTION
```

**Complexity:** O(k)[9]

***

**False Positive Rate:**
```text
FUNCTION estimateFalsePositiveRate()
    // Formula: (1 - e^(-kn/m))^k
    exponent = (-k * n) / m
    probability = (1 - EXP(exponent)) ^ k
    RETURN probability
END FUNCTION

FUNCTION getCurrentFillRatio()
    setBits = 0
    FOR i = 0 TO m-1 DO
        IF bits[i] == 1 THEN
            setBits += 1
        END IF
    END FOR
    RETURN setBits / m
END FUNCTION
```

***

### Counting Bloom Filter

**Structure:**
```text
CLASS CountingBloomFilter
    counters = ARRAY[m]  // Array of counters (not bits)
    k = number of hash functions
    m = array size
    
    FUNCTION init(size, hashCount)
        m = size
        k = hashCount
        counters = ARRAY[m] filled with 0
    END FUNCTION
END CLASS
```

**Operations:**
```text
FUNCTION add(element)
    FOR i = 0 TO k-1 DO
        index = getHashValue(element, i)
        counters[index] += 1
    END FOR
END FUNCTION

FUNCTION remove(element)
    // Check if element might exist
    IF NOT contains(element) THEN
        RETURN false
    END IF
    
    FOR i = 0 TO k-1 DO
        index = getHashValue(element, i)
        IF counters[index] > 0 THEN
            counters[index] -= 1
        END IF
    END FOR
    RETURN true
END FUNCTION

FUNCTION contains(element)
    FOR i = 0 TO k-1 DO
        index = getHashValue(element, i)
        IF counters[index] == 0 THEN
            RETURN false
        END IF
    END FOR
    RETURN true
END FUNCTION
```

***

## 5. LRU Cache Pseudocode

### LRU Cache with Hash Map + Doubly Linked List

**Node Structure:**
```text
CLASS Node
    key
    value
    prev = null
    next = null
    
    FUNCTION init(k, v)
        key = k
        value = v
    END FUNCTION
END CLASS
```

**LRU Cache Structure:**
```text
CLASS LRUCache
    capacity
    cache = HASHMAP()  // key → Node
    head = Node(-1, -1)  // Dummy head (MRU end)
    tail = Node(-1, -1)  // Dummy tail (LRU end)
    size = 0
    
    FUNCTION init(cap)
        capacity = cap
        head.next = tail
        tail.prev = head
    END FUNCTION
END CLASS
```

***

**Get Operation:**
```text
FUNCTION get(key)
    IF key NOT IN cache THEN
        RETURN -1
    END IF
    
    node = cache[key]
    
    // Move to front (most recently used)
    moveToHead(node)
    
    RETURN node.value
END FUNCTION
```

**Complexity:** O(1)[10]

***

**Put Operation:**
```text
FUNCTION put(key, value)
    IF key IN cache THEN
        // Update existing key
        node = cache[key]
        node.value = value
        moveToHead(node)
    ELSE
        // Add new key
        IF size >= capacity THEN
            // Evict LRU
            lruNode = tail.prev
            removeNode(lruNode)
            DELETE cache[lruNode.key]
            size -= 1
        END IF
        
        newNode = Node(key, value)
        cache[key] = newNode
        addToHead(newNode)
        size += 1
    END IF
END FUNCTION
```

**Complexity:** O(1)[10]

***

**Helper Functions:**
```text
FUNCTION addToHead(node)
    node.next = head.next
    node.prev = head
    head.next.prev = node
    head.next = node
END FUNCTION

FUNCTION removeNode(node)
    node.prev.next = node.next
    node.next.prev = node.prev
END FUNCTION

FUNCTION moveToHead(node)
    removeNode(node)
    addToHead(node)
END FUNCTION

FUNCTION removeTail()
    lru = tail.prev
    removeNode(lru)
    RETURN lru
END FUNCTION
```

***

## 6. Trie (Prefix Tree) Pseudocode

### Basic Trie

**Node Structure:**
```text
CLASS TrieNode
    children = ARRAY[26]  // For lowercase a-z
    isEndOfWord = false
    
    FUNCTION init()
        FOR i = 0 TO 25 DO
            children[i] = null
        END FOR
        isEndOfWord = false
    END FUNCTION
END CLASS
```

**Trie Structure:**
```text
CLASS Trie
    root = TrieNode()
    
    FUNCTION init()
        root = TrieNode()
    END FUNCTION
END CLASS
```

***

**Insert:**
```text
FUNCTION insert(word)
    node = root
    
    FOR char IN word DO
        index = char - 'a'
        
        IF children[index] == null THEN
            children[index] = TrieNode()
        END IF
        
        node = children[index]
    END FOR
    
    node.isEndOfWord = true
END FUNCTION
```

**Complexity:** O(m) where m = word length[11]

***

**Search:**
```text
FUNCTION search(word)
    node = searchPrefix(word)
    RETURN node != null AND node.isEndOfWord
END FUNCTION

FUNCTION searchPrefix(prefix)
    node = root
    
    FOR char IN prefix DO
        index = char - 'a'
        
        IF children[index] == null THEN
            RETURN null
        END IF
        
        node = children[index]
    END FOR
    
    RETURN node
END FUNCTION
```

**Complexity:** O(m)[11]

***

**Starts With:**
```text
FUNCTION startsWith(prefix)
    RETURN searchPrefix(prefix) != null
END FUNCTION
```

***

**Delete:**
```text
FUNCTION delete(word)
    RETURN deleteHelper(root, word, 0)
END FUNCTION

FUNCTION deleteHelper(node, word, depth)
    IF node == null THEN
        RETURN false
    END IF
    
    // Reached end of word
    IF depth == length(word) THEN
        // Word doesn't exist
        IF NOT node.isEndOfWord THEN
            RETURN false
        END IF
        
        node.isEndOfWord = false
        
        // Check if node has no children
        RETURN hasNoChildren(node)
    END IF
    
    index = word[depth] - 'a'
    shouldDeleteChild = deleteHelper(node.children[index], word, depth + 1)
    
    IF shouldDeleteChild THEN
        node.children[index] = null
        
        // If current node has no other children and is not end of another word
        RETURN NOT node.isEndOfWord AND hasNoChildren(node)
    END IF
    
    RETURN false
END FUNCTION

FUNCTION hasNoChildren(node)
    FOR i = 0 TO 25 DO
        IF children[i] != null THEN
            RETURN false
        END IF
    END FOR
    RETURN true
END FUNCTION
```

***

### Trie with Frequency Count

**Node Structure:**
```text
CLASS TrieNodeWithFreq
    children = MAP()  // Character → Node
    isEndOfWord = false
    frequency = 0
    word = ""
END CLASS
```

**Insert with Frequency:**
```text
FUNCTION insertWithFrequency(word, freq)
    node = root
    
    FOR char IN word DO
        IF char NOT IN node.children THEN
            node.children[char] = TrieNodeWithFreq()
        END IF
        node = node.children[char]
    END FOR
    
    node.isEndOfWord = true
    node.frequency += freq
    node.word = word
END FUNCTION
```

***

## 7. Advanced: Persistent Segment Tree

**Node Structure:**
```text
CLASS PersistentNode
    value
    left = null
    right = null
    
    FUNCTION init(val)
        value = val
    END FUNCTION
END CLASS
```

**Build:**
```text
FUNCTION buildPersistent(arr, left, right)
    IF left == right THEN
        node = PersistentNode(arr[left])
        RETURN node
    END IF
    
    mid = (left + right) / 2
    node = PersistentNode(0)
    node.left = buildPersistent(arr, left, mid)
    node.right = buildPersistent(arr, mid+1, right)
    node.value = node.left.value + node.right.value
    
    RETURN node
END FUNCTION
```

**Update (Creates New Version):**
```text
FUNCTION updatePersistent(oldNode, left, right, index, newValue)
    IF left == right THEN
        newNode = PersistentNode(newValue)
        RETURN newNode
    END IF
    
    mid = (left + right) / 2
    newNode = PersistentNode(0)
    
    IF index <= mid THEN
        newNode.left = updatePersistent(oldNode.left, left, mid, index, newValue)
        newNode.right = oldNode.right  // Reuse old subtree
    ELSE
        newNode.left = oldNode.left  // Reuse old subtree
        newNode.right = updatePersistent(oldNode.right, mid+1, right, index, newValue)
    END IF
    
    newNode.value = newNode.left.value + newNode.right.value
    RETURN newNode
END FUNCTION
```

***

## Implementation Complexity Summary

| Data Structure | Build | Query | Update | Space |
|----------------|-------|-------|--------|-------|
| Segment Tree | O(n) | O(log n) | O(log n) | O(4n) |
| Segment Tree (Lazy) | O(n) | O(log n) | O(log n) | O(4n) |
| Fenwick Tree | O(n log n) | O(log n) | O(log n) | O(n) |
| Union-Find | O(n) | O(α(n)) | O(α(n)) | O(n) |
| Bloom Filter | O(k) per insert | O(k) | N/A | O(m) bits |
| LRU Cache | O(1) per op | O(1) | O(1) | O(capacity) |
| Trie | O(total chars) | O(m) | O(m) | O(ALPHABET × N × M) |

[6][8][1][5][2][3][4]

***

**These pseudocode implementations provide battle-tested foundations for solving complex algorithmic problems. Master these patterns to build robust, efficient solutions in interviews and production systems!**[8][1][2][3][4]

[1](https://cp-algorithms.com/data_structures/segment_tree.html)
[2](https://www.geeksforgeeks.org/dsa/introduction-to-disjoint-set-data-structure-or-union-find-algorithm/)
[3](https://en.wikipedia.org/wiki/Fenwick_tree)
[4](https://www.geeksforgeeks.org/dsa/binary-indexed-tree-or-fenwick-tree-2/)
[5](https://www.geeksforgeeks.org/dsa/introduction-to-segment-trees-2/)
[6](https://en.algorithmica.org/hpc/data-structures/segment-trees/)
[7](https://usaco.guide/adv/segtree-beats)
[8](https://www.geeksforgeeks.org/dsa/union-by-rank-and-path-compression-in-union-find-algorithm/)
[9](https://www.upgrad.com/blog/bloom-filters-in-data-structure/)
[10](https://www.geeksforgeeks.org/system-design/lru-cache-implementation/)
[11](https://www.educative.io/blog/data-structures-tutorial-advanced)
[12](https://codeforces.com/blog/entry/15890)
[13](https://www.sciencedirect.com/science/article/pii/S2215016119300391)