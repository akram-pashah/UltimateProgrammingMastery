I see the files are about Sets & Maps, not Advanced Data Structures. Let me create a comprehensive guide for Advanced Data Structures based on the web research and create content similar to the structure you've been using.

# 🔥 Advanced Data Structures: Complete Mastery from Foundations to Expert Level

Advanced data structures are specialized, high-performance structures designed to solve complex computational problems efficiently. They extend beyond basic arrays, linked lists, stacks, and queues to handle sophisticated operations like range queries, dynamic connectivity, string processing, and probabilistic membership testing.[1][2][3][4][5][6]

## What Are Advanced Data Structures?

Advanced data structures are optimized solutions for specific problem domains where basic structures become inefficient. They typically offer logarithmic or near-constant time complexity for operations that would otherwise require linear time.[2][3][4][5][1]

**Key Characteristics:**
- **Specialized operations**: Support complex queries (range sum, minimum, maximum)[4][1]
- **Optimal complexity**: O(log n) or better for most operations[3][5][2]
- **Space-time tradeoffs**: Often use additional space for performance gains[6][7]
- **Domain-specific**: Designed for particular problem types (strings, graphs, ranges)[8][9]

---

## Segment Trees

### Fundamentals

A **Segment Tree** is a binary tree data structure that stores information about array intervals, enabling efficient range queries and updates.[5][1][4]

**Core Properties:**
- Tree representation of array intervals[1]
- Each node stores aggregate information (sum, min, max, GCD)[5][1]
- Height is O(log n) for array of size n[1]
- Space complexity: O(4n) ≈ O(n)[4][5]

**Time Complexity:**
- Build: O(n)[4][1]
- Range query: O(log n)[5][1]
- Point update: O(log n)[1][5]
- Range update (with lazy propagation): O(log n)[4][1]

### Structure

```text
Array: [1, 3, 5, 7, 9, 11]
Segment Tree (sum):
           36 (0-5)
          /         \
       9 (0-2)      27 (3-5)
      /      \      /      \
   4(0-1)  5(2)  16(3-4) 11(5)
   /  \         /   \
  1(0) 3(1)   7(3) 9(4)
```

### Operations

**Range Sum Query:**
```text
FUNCTION rangeSum(node, L, R, queryL, queryR)
    // No overlap
    IF R < queryL OR L > queryR THEN
        RETURN 0
    END IF
    
    // Complete overlap
    IF queryL <= L AND R <= queryR THEN
        RETURN tree[node]
    END IF
    
    // Partial overlap
    mid = (L + R) / 2
    leftSum = rangeSum(2*node, L, mid, queryL, queryR)
    rightSum = rangeSum(2*node+1, mid+1, R, queryL, queryR)
    RETURN leftSum + rightSum
END FUNCTION
```

**Point Update:**
```text
FUNCTION update(node, L, R, index, newValue)
    IF L == R THEN
        tree[node] = newValue
        RETURN
    END IF
    
    mid = (L + R) / 2
    IF index <= mid THEN
        update(2*node, L, mid, index, newValue)
    ELSE
        update(2*node+1, mid+1, R, index, newValue)
    END IF
    
    tree[node] = tree[2*node] + tree[2*node+1]
END FUNCTION
```

**Lazy Propagation (Range Updates):**
```text
FUNCTION rangeUpdate(node, L, R, queryL, queryR, delta)
    // Apply pending updates
    IF lazy[node] != 0 THEN
        tree[node] += (R - L + 1) * lazy[node]
        IF L != R THEN
            lazy[2*node] += lazy[node]
            lazy[2*node+1] += lazy[node]
        END IF
        lazy[node] = 0
    END IF
    
    // No overlap
    IF R < queryL OR L > queryR THEN
        RETURN
    END IF
    
    // Complete overlap
    IF queryL <= L AND R <= queryR THEN
        tree[node] += (R - L + 1) * delta
        IF L != R THEN
            lazy[2*node] += delta
            lazy[2*node+1] += delta
        END IF
        RETURN
    END IF
    
    // Partial overlap
    mid = (L + R) / 2
    rangeUpdate(2*node, L, mid, queryL, queryR, delta)
    rangeUpdate(2*node+1, mid+1, R, queryL, queryR, delta)
    tree[node] = tree[2*node] + tree[2*node+1]
END FUNCTION
```

### Applications

- Range sum/min/max queries[5][1]
- Range GCD/LCM queries[1]
- Finding k-th smallest element in range[1]
- Counting inversions[1]
- Geometric problems (line sweep algorithms)[1]

---

## Fenwick Tree (Binary Indexed Tree)

### Fundamentals

A **Fenwick Tree** or **Binary Indexed Tree (BIT)** efficiently supports prefix sum queries and point updates using clever bit manipulation.[2][3][4]

**Core Properties:**
- Array-based implementation[3][2]
- Uses bit manipulation for parent-child relationships[3]
- Space complexity: O(n)[3][5]
- Simpler than segment trees for prefix operations[3][5]

**Time Complexity:**
- Build: O(n log n)[2][3]
- Prefix sum: O(log n)[2][3]
- Point update: O(log n)[2][3]
- Range sum: O(log n) (two prefix sums)[5][3]

### Structure

```text
Array:     [1, 3, 5, 7, 9, 11]
BIT:       [0, 1, 4, 5, 16, 9, 20]
           
Index in binary:
1  = 0001 → stores sum of 1 element
2  = 0010 → stores sum of 2 elements  
3  = 0011 → stores sum of 1 element
4  = 0100 → stores sum of 4 elements
```

### Operations

**Prefix Sum:**
```text
FUNCTION prefixSum(index)
    sum = 0
    WHILE index > 0 DO
        sum += BIT[index]
        index -= (index & -index)  // Remove last set bit
    END WHILE
    RETURN sum
END FUNCTION
```

**Point Update:**
```text
FUNCTION update(index, delta)
    WHILE index <= n DO
        BIT[index] += delta
        index += (index & -index)  // Add last set bit
    END WHILE
END FUNCTION
```

**Range Sum:**
```text
FUNCTION rangeSum(left, right)
    RETURN prefixSum(right) - prefixSum(left - 1)
END FUNCTION
```

**Build BIT:**
```text
FUNCTION buildBIT(arr)
    BIT = ARRAY[n+1] filled with 0
    FOR i = 1 TO n DO
        update(i, arr[i])
    END FOR
END FUNCTION
```

### Applications

- Prefix sums[2][3][5]
- Frequency tables[3][5]
- Counting inversions[2]
- 2D range sum queries[2]
- Competitive programming optimization[5][3]

### Comparison: Segment Tree vs Fenwick Tree

| Feature | Segment Tree | Fenwick Tree |
|---------|--------------|--------------|
| Query Types | Range sum/min/max/GCD | Prefix sum primarily |
| Update Types | Point + Range (lazy) | Point (easily) |
| Space | O(4n) | O(n) |
| Implementation | Complex | Simple |
| Custom Operations | Supported | Limited |
| Code Size | Larger | Smaller |

[4][5]

***

## Union-Find (Disjoint Set Union)

### Fundamentals

**Union-Find** (also called Disjoint Set Union or DSU) efficiently manages a partition of elements into disjoint sets, supporting union and find operations.[7][1]

**Core Operations:**
- **find(x)**: Determine which set x belongs to[7]
- **union(x, y)**: Merge sets containing x and y[7]
- **isConnected(x, y)**: Check if x and y are in same set[7]

**Time Complexity:**
- Without optimization: O(n) per operation
- With path compression: O(log n)
- With union by rank: O(log n)
- With both optimizations: O(α(n)) ≈ O(1) amortized[7]

α(n) is the inverse Ackermann function, which grows extremely slowly (≤ 5 for all practical values of n).[7]

### Implementation

**Basic Structure:**
```text
CLASS UnionFind
    parent = ARRAY[n]
    rank = ARRAY[n]
    
    FUNCTION makeSet(x)
        parent[x] = x
        rank[x] = 0
    END FUNCTION
    
    FUNCTION find(x)
        // Path compression
        IF parent[x] != x THEN
            parent[x] = find(parent[x])
        END IF
        RETURN parent[x]
    END FUNCTION
    
    FUNCTION union(x, y)
        rootX = find(x)
        rootY = find(y)
        
        IF rootX == rootY THEN
            RETURN  // Already in same set
        END IF
        
        // Union by rank
        IF rank[rootX] < rank[rootY] THEN
            parent[rootX] = rootY
        ELSE IF rank[rootX] > rank[rootY] THEN
            parent[rootY] = rootX
        ELSE
            parent[rootY] = rootX
            rank[rootX] += 1
        END IF
    END FUNCTION
    
    FUNCTION isConnected(x, y)
        RETURN find(x) == find(y)
    END FUNCTION
    
    FUNCTION countComponents()
        roots = SET
        FOR i = 0 TO n-1 DO
            roots.add(find(i))
        END FOR
        RETURN size(roots)
    END FUNCTION
END CLASS
```

### Applications

- Kruskal's Minimum Spanning Tree algorithm[7]
- Detecting cycles in undirected graphs[7]
- Network connectivity problems[7]
- Image segmentation[7]
- Finding connected components[7]
- Dynamic connectivity queries

***

## Bloom Filter

### Fundamentals

A **Bloom Filter** is a space-efficient probabilistic data structure for testing set membership. It can have **false positives** but never false negatives.[6]

**Core Properties:**
- Space-efficient: uses bit array[6]
- Probabilistic: may report element exists when it doesn't[6]
- No false negatives: if it says "not present", it's definitely not present[6]
- Cannot remove elements (standard version)[6]

**Time Complexity:**
- Add: O(k) where k = number of hash functions[6]
- Contains: O(k)[6]

**Space Complexity:**
- O(m) bits, where m is size of bit array[6]
- Much smaller than hash set for large datasets[6]

### Implementation

```text
CLASS BloomFilter
    bits = BITARRAY(m)
    m = size of bit array
    k = number of hash functions
    
    FUNCTION add(element)
        FOR i = 0 TO k-1 DO
            index = hash_i(element) % m
            bits[index] = 1
        END FOR
    END FUNCTION
    
    FUNCTION contains(element)
        FOR i = 0 TO k-1 DO
            index = hash_i(element) % m
            IF bits[index] == 0 THEN
                RETURN false  // Definitely NOT present
            END IF
        END FOR
        RETURN true  // Probably present (may be false positive)
    END FUNCTION
    
    FUNCTION expectedFalsePositiveRate()
        // Formula: (1 - e^(-kn/m))^k
        // n = number of inserted elements
        RETURN (1 - exp(-k * n / m)) ^ k
    END FUNCTION
END CLASS
```

### Optimal Parameters

**Optimal number of hash functions:**
$$ k = \frac{m}{n} \ln(2) $$

**Optimal bit array size for target false positive rate p:**
$$ m = -\frac{n \ln(p)}{(\ln(2))^2} $$
[6]

### Applications

- Spell checkers[6]
- Web browsers (malicious URL detection)[6]
- Database query optimization[6]
- Distributed caching (avoiding cache misses)[6]
- Network security (DDoS protection)[6]
- Blockchain (Bitcoin uses Bloom filters)[6]

***

## LRU Cache

### Fundamentals

An **LRU (Least Recently Used) Cache** evicts the least recently used item when capacity is reached. It combines a hash map with a doubly-linked list for O(1) operations.[10]

**Structure:**
- Hash map: key → node pointer[10]
- Doubly-linked list: maintains access order[10]
- Head: most recently used (MRU)[10]
- Tail: least recently used (LRU)[10]

**Time Complexity:**
- get: O(1)[10]
- put: O(1)[10]

**Space Complexity:**
- O(capacity)[10]

### Implementation

```text
CLASS Node
    key
    value
    prev
    next
    
    FUNCTION init(key, value)
        THIS.key = key
        THIS.value = value
    END FUNCTION
END CLASS

CLASS LRUCache
    capacity
    cache = HashMap()  // key → node
    head = Node(-1, -1)  // Dummy head
    tail = Node(-1, -1)  // Dummy tail
    
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
                // Evict LRU
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

### Applications

- Web browser caching[10]
- Database buffer pool management[10]
- CPU cache replacement policies[10]
- Operating system page replacement[10]
- CDN content caching[10]

---

## Trie (Prefix Tree)

### Fundamentals

A **Trie** is a tree-like data structure for storing strings, where each node represents a character and paths represent words.[7]

**Core Properties:**
- Root represents empty string
- Each path from root to leaf represents a word
- Common prefixes share nodes
- Space-efficient for storing many strings with common prefixes

**Time Complexity:**
- Insert: O(m) where m = word length
- Search: O(m)
- StartsWith: O(m)

**Space Complexity:**
- O(ALPHABET_SIZE × N × M) worst case
- Much better with compression

### Implementation

```text
CLASS TrieNode
    children = ARRAY[26]  // for lowercase a-z
    isEndOfWord = false
END CLASS

CLASS Trie
    root = TrieNode()
    
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
    
    FUNCTION search(word)
        node = searchPrefix(word)
        RETURN node != null AND node.isEndOfWord
    END FUNCTION
    
    FUNCTION startsWith(prefix)
        RETURN searchPrefix(prefix) != null
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
    
    FUNCTION delete(word)
        deleteHelper(root, word, 0)
    END FUNCTION
    
    FUNCTION deleteHelper(node, word, depth)
        IF node == null THEN
            RETURN false
        END IF
        
        IF depth == length(word) THEN
            IF NOT node.isEndOfWord THEN
                RETURN false
            END IF
            node.isEndOfWord = false
            RETURN hasNoChildren(node)
        END IF
        
        index = word[depth] - 'a'
        IF deleteHelper(node.children[index], word, depth + 1) THEN
            node.children[index] = null
            RETURN NOT node.isEndOfWord AND hasNoChildren(node)
        END IF
        
        RETURN false
    END FUNCTION
END CLASS
```

### Applications

- Autocomplete systems
- Spell checkers
- IP routing (longest prefix matching)
- Dictionary implementations
- Word games (Boggle, Scrabble)
- Contact search in phones

***

## Suffix Array and Suffix Tree

### Suffix Array

A **suffix array** is a sorted array of all suffixes of a string, represented by their starting indices.[9][8]

**Example:**
```text
String: "banana"
Suffixes:
  0: banana
  1: anana
  2: nana
  3: ana
  4: na
  5: a

Suffix Array (sorted): [5, 3, 1, 0, 4, 2]
  a
  ana
  anana
  banana
  na
  nana
```

**Construction:**
- Naive: O(n² log n)[8]
- Optimized: O(n log n)[8]
- Advanced (DC3, SA-IS): O(n)[8]

**Applications:**
- Pattern matching in O(m log n) where m = pattern length[8]
- Finding longest common substring[8]
- Data compression[8]
- Bioinformatics (DNA sequence analysis)[8]

### Suffix Tree

A **suffix tree** is a compressed trie of all suffixes of a string.[9][8]

**Properties:**
- Exactly n leaves for string of length n[8]
- Each edge labeled with non-empty substring[8]
- Every internal node has at least 2 children[8]
- Can be constructed in O(n) time (Ukkonen's algorithm)[8]

**Applications:**
- Pattern matching in O(m) time[8]
- Finding longest repeated substring[8]
- Longest common substring of multiple strings[8]
- DNA sequence alignment[8]

---

## Rope Data Structure

### Fundamentals

A **Rope** is a tree-based data structure for efficiently storing and manipulating very long strings.[7]

**Structure:**
- Binary tree where leaves contain string segments
- Internal nodes store length of left subtree
- Balanced for O(log n) operations

**Time Complexity:**
- Concatenate: O(1) or O(log n) with rebalancing
- Split: O(log n)
- Insert/Delete: O(log n)
- Index: O(log n)

**Advantages over strings:**
- Efficient concatenation (no copying)
- Efficient insertion/deletion in middle
- Undo/redo operations
- Good for text editors with large documents

### Applications

- Text editors (handling large documents)
- String manipulation in functional languages
- Version control systems
- Collaborative editing

***

## Specialized Structures Summary

### LFU Cache

**Least Frequently Used Cache** evicts items with lowest access frequency.

**Implementation:**
- HashMap: key → {value, frequency}
- HashMap: frequency → doubly-linked list of keys
- Min frequency tracker

**Time Complexity:** O(1) for get and put

### Skip List

Probabilistic data structure that allows O(log n) search in sorted list.

**Properties:**
- Multiple levels of linked lists
- Level 0 contains all elements
- Higher levels skip elements probabilistically
- Alternative to balanced trees

### Treap

Combination of binary search tree and heap.

**Properties:**
- Each node has key (BST property) and priority (heap property)
- Randomized priorities give balanced tree with high probability
- Simpler than AVL or Red-Black trees

### Splay Tree

Self-adjusting binary search tree.

**Properties:**
- Recently accessed elements near root (locality)
- No explicit balance information stored
- Amortized O(log n) operations

***

## Advanced Topics

### Persistent Data Structures

Data structures that preserve previous versions when modified.

**Techniques:**
- Path copying
- Fat nodes
- Version arrays

**Applications:**
- Version control
- Functional programming
- Undo/redo systems

### Succinct Data Structures

Achieve information-theoretic lower bounds on space.

**Examples:**
- Rank/Select on bit vectors
- Wavelet trees
- Compressed suffix arrays

### Cache-Oblivious Data Structures

Designed to be efficient across all levels of memory hierarchy without knowing cache parameters.

**Examples:**
- Van Emde Boas layout
- Cache-oblivious B-trees

***

## Mastery Roadmap

### Level 1: Foundations (Weeks 1-2)
- Understand segment trees and Fenwick trees
- Implement basic range query operations
- Practice Union-Find with path compression

### Level 2: Intermediate (Weeks 3-4)
- Lazy propagation in segment trees
- 2D range queries
- Trie implementation and applications

### Level 3: Advanced (Weeks 5-6)
- Bloom filters and probabilistic structures
- LRU/LFU cache designs
- Suffix arrays and trees

### Level 4: Expert (Weeks 7-8)
- Persistent data structures
- Advanced string algorithms
- Specialized structures (rope, skip list)

### Level 5: Mastery (Weeks 9-10)
- System design with advanced structures
- Performance optimization
- Competitive programming applications

***

## Essential Skills Checklist

✅ Implement segment tree from scratch  
✅ Use Fenwick tree for prefix sums  
✅ Apply Union-Find to graph problems  
✅ Build Bloom filter with optimal parameters  
✅ Design LRU cache with O(1) operations  
✅ Implement Trie for string operations  
✅ Understand suffix array construction  
✅ Choose right structure for problem domain  
✅ Optimize for space-time tradeoffs  
✅ Apply to competitive programming problems  

***

**Advanced data structures are the secret weapons of top engineers—powering high-performance systems, winning competitive programming contests, and solving problems that seem impossible with basic structures. Master them to unlock the next level of algorithmic thinking!**[9][4][3][5][1][2][7][10][8][6]

[1](https://cp-algorithms.com/data_structures/segment_tree.html)
[2](https://www.geeksforgeeks.org/dsa/binary-indexed-tree-or-fenwick-tree-2/)
[3](https://www.shadecoder.com/topics/what-is-fenwick-tree-bit-a-practical-guide-for-2025)
[4](https://jjv.ie/slides/segment-tree.pdf)
[5](https://www.nullpointerclub.com/p/demystifying-data-structures-one-tree-at-a-time)
[6](https://www.upgrad.com/blog/bloom-filters-in-data-structure/)
[7](https://www.educative.io/blog/data-structures-tutorial-advanced)
[8](https://www.scholarhat.com/tutorial/datastructures/suffix-array-and-suffix-tree-in-data-structures)
[9](http://web.cs.iastate.edu/~cs548/suffix.pdf)
[10](https://www.geeksforgeeks.org/system-design/lru-cache-implementation/)
[11](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/87948039/2b796f31-20e3-4b19-86b1-5fcc77d2e2cc/DESCRIPTION.md)
[12](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/87948039/c31f7f06-ad5a-480f-b4b0-24d76862d54c/PSEUDOCODE.md)
[13](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/87948039/a08b87dd-e91d-474d-bc42-a86173ba45ba/EXAMPLES.md)