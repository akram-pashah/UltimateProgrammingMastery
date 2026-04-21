# 🔥 Examples: Advanced Data Structures - Essential, Advanced, and FAANG-Level Mastery

This comprehensive guide covers real-world coding examples for advanced data structures, progressing from basic implementations to complex interview problems asked at top tech companies.[1][2][3][4]

***

## 1. Segment Tree Examples

### Basic: Range Sum Query

**Problem:** Given an array, support efficient range sum queries and point updates.[5][3]

```text
FUNCTION rangeSum(arr, queries, updates)
    Build segment tree from arr
    FOR each query (L, R) DO
        result = querySumTree(L, R)
        PRINT "Sum of range [" + L + ", " + R + "] = " + result
    END FOR
END FUNCTION

Example:
arr = [1, 3, 5, 7, 9, 11]
query(1, 3) → 3 + 5 + 7 = 15
update(1, 10)
query(1, 3) → 10 + 5 + 7 = 22
```

**Complexity:** O(n) build, O(log n) per query/update[3]

***

### Intermediate: Range Minimum Query (RMQ)

**Problem:** Find minimum element in any range and support updates.[2][3]

```text
CLASS SegmentTreeMin
    tree = ARRAY[4*n]
    
    FUNCTION buildTree(arr, node, L, R)
        IF L == R THEN
            tree[node] = arr[L]
            RETURN tree[node]
        END IF
        
        mid = (L + R) / 2
        leftMin = buildTree(arr, 2*node, L, mid)
        rightMin = buildTree(arr, 2*node+1, mid+1, R)
        tree[node] = MIN(leftMin, rightMin)
        RETURN tree[node]
    END FUNCTION
    
    FUNCTION queryMin(node, L, R, queryL, queryR)
        // No overlap
        IF R < queryL OR L > queryR THEN
            RETURN INFINITY
        END IF
        
        // Complete overlap
        IF queryL <= L AND R <= queryR THEN
            RETURN tree[node]
        END IF
        
        // Partial overlap
        mid = (L + R) / 2
        leftMin = queryMin(2*node, L, mid, queryL, queryR)
        rightMin = queryMin(2*node+1, mid+1, R, queryL, queryR)
        RETURN MIN(leftMin, rightMin)
    END FUNCTION
    
    FUNCTION updatePoint(node, L, R, index, newValue)
        IF L == R THEN
            tree[node] = newValue
            RETURN
        END IF
        
        mid = (L + R) / 2
        IF index <= mid THEN
            updatePoint(2*node, L, mid, index, newValue)
        ELSE
            updatePoint(2*node+1, mid+1, R, index, newValue)
        END IF
        
        tree[node] = MIN(tree[2*node], tree[2*node+1])
    END FUNCTION
END CLASS

Example:
arr = [2, 5, 1, 4, 9, 3]
queryMin(1, 4) → MIN(5, 1, 4, 9) = 1
update(2, 8)  // arr[2] = 1 → 8
queryMin(1, 4) → MIN(5, 8, 4, 9) = 4
```

**Complexity:** O(n) build, O(log n) per operation[2][3]

***

### Advanced: Range Update with Lazy Propagation

**Problem:** Add a value to all elements in a range efficiently.[3][2]

```text
CLASS LazySegmentTree
    tree = ARRAY[4*n]    // Stores sum
    lazy = ARRAY[4*n]    // Stores pending updates
    
    FUNCTION rangeUpdate(node, L, R, queryL, queryR, delta)
        // Apply pending updates first
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
    
    FUNCTION rangeQuery(node, L, R, queryL, queryR)
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
            RETURN 0
        END IF
        
        // Complete overlap
        IF queryL <= L AND R <= queryR THEN
            RETURN tree[node]
        END IF
        
        // Partial overlap
        mid = (L + R) / 2
        leftSum = rangeQuery(2*node, L, mid, queryL, queryR)
        rightSum = rangeQuery(2*node+1, mid+1, R, queryL, queryR)
        RETURN leftSum + rightSum
    END FUNCTION
END CLASS

Example:
arr = [1, 3, 5, 7, 9]
rangeUpdate(1, 3, 5)  // Add 5 to arr[1..3]
// arr becomes [1, 8, 10, 12, 9]
rangeQuery(0, 4) → 40
```

**Complexity:** O(log n) per operation with lazy propagation[2][3]

***

### FAANG Problem: Count of Range Sum (LeetCode 327)

**Problem:** Count number of range sums in [lower, upper].[6][2]

```text
FUNCTION countRangeSum(nums, lower, upper)
    n = length(nums)
    prefixSum = ARRAY[n+1] where prefixSum[0] = 0
    
    // Build prefix sum array
    FOR i = 1 TO n DO
        prefixSum[i] = prefixSum[i-1] + nums[i-1]
    END FOR
    
    // Use segment tree to count ranges
    count = 0
    sorted = SORT(prefixSum)
    segTree = SegmentTree(sorted)
    
    FOR i = 0 TO n DO
        // Count how many j < i satisfy:
        // lower <= prefixSum[i] - prefixSum[j] <= upper
        // i.e., prefixSum[i] - upper <= prefixSum[j] <= prefixSum[i] - lower
        
        leftBound = prefixSum[i] - upper
        rightBound = prefixSum[i] - lower
        count += segTree.query(leftBound, rightBound)
        segTree.update(prefixSum[i])
    END FOR
    
    RETURN count
END FUNCTION

Example:
nums = [-2, 5, -1], lower = -2, upper = 2
prefixSum = [0, -2, 3, 2]
Count ranges: [-2,5]=3, [5,-1]=4, [-2,5,-1]=2, [-2]=−2, [5]=5, [-1]=−1
Result: 3 (ranges with sum in [-2, 2])
```

**Complexity:** O(n log n)[6][2]

---

## 2. Fenwick Tree (BIT) Examples

### Basic: Prefix Sum Queries

**Problem:** Compute prefix sum and handle point updates efficiently.[7][8]

```text
CLASS FenwickTree
    BIT = ARRAY[n+1] filled with 0
    
    FUNCTION update(index, delta)
        // index is 1-indexed
        WHILE index <= n DO
            BIT[index] += delta
            index += (index & -index)  // Add last set bit
        END WHILE
    END FUNCTION
    
    FUNCTION prefixSum(index)
        sum = 0
        WHILE index > 0 DO
            sum += BIT[index]
            index -= (index & -index)  // Remove last set bit
        END WHILE
        RETURN sum
    END FUNCTION
    
    FUNCTION rangeSum(left, right)
        RETURN prefixSum(right) - prefixSum(left - 1)
    END FUNCTION
    
    FUNCTION build(arr)
        FOR i = 0 TO length(arr)-1 DO
            update(i+1, arr[i])
        END FOR
    END FUNCTION
END CLASS

Example:
arr = [3, 2, -1, 6, 5, 4, -3, 3, 7, 2, 3]
prefixSum(5) → 15
update(3, 6)  // arr[2] = -1 → 5
prefixSum(5) → 21
rangeSum(2, 5) → 18
```

**Complexity:** O(log n) per operation[8][7]

---

### Intermediate: 2D Range Sum Query

**Problem:** Compute sum of any rectangle in 2D matrix.[7][8]

```text
CLASS FenwickTree2D
    BIT = 2D_ARRAY[rows+1][cols+1]
    
    FUNCTION update(row, col, delta)
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
    
    FUNCTION query(row, col)
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
    
    FUNCTION rangeSum(row1, col1, row2, col2)
        RETURN query(row2, col2) 
             - query(row1-1, col2)
             - query(row2, col1-1)
             + query(row1-1, col1-1)
    END FUNCTION
END CLASS

Example:
matrix = [[1, 2, 3],
          [4, 5, 6],
          [7, 8, 9]]
rangeSum(1, 1, 2, 2) → 5 + 6 + 8 + 9 = 28
```

**Complexity:** O(log m × log n) per operation[8][7]

***

### Advanced: Count of Smaller Numbers After Self

**Problem:** For each element, count how many smaller elements appear after it.[7][8]

```text
FUNCTION countSmaller(nums)
    n = length(nums)
    result = ARRAY[n]
    
    // Coordinate compression
    sorted = SORT(SET(nums))
    compress = MAP sorted values to ranks 1..k
    
    fenwick = FenwickTree(k)
    
    // Process from right to left
    FOR i = n-1 DOWN TO 0 DO
        rank = compress[nums[i]]
        result[i] = fenwick.prefixSum(rank - 1)
        fenwick.update(rank, 1)
    END FOR
    
    RETURN result
END FUNCTION

Example:
nums = [5, 2, 6, 1]
Process:
  i=3: nums[3]=1, rank=1, count=0, add 1
  i=2: nums[2]=6, rank=4, count=2, add 6
  i=1: nums[1]=2, rank=2, count=1, add 2
  i=0: nums[0]=5, rank=3, count=2, add 5
Result: [2, 1, 1, 0]
```

**Complexity:** O(n log n)[8][7]

***

## 3. Union-Find Examples

### Basic: Connected Components

**Problem:** Determine if two nodes are connected in a graph.[4][9]

```text
CLASS UnionFind
    parent = ARRAY[n]
    rank = ARRAY[n]
    
    FUNCTION makeSet()
        FOR i = 0 TO n-1 DO
            parent[i] = i
            rank[i] = 0
        END FOR
    END FUNCTION
    
    FUNCTION find(x)
        IF parent[x] != x THEN
            parent[x] = find(parent[x])  // Path compression
        END IF
        RETURN parent[x]
    END FUNCTION
    
    FUNCTION union(x, y)
        rootX = find(x)
        rootY = find(y)
        
        IF rootX == rootY THEN
            RETURN false  // Already connected
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
        
        RETURN true
    END FUNCTION
    
    FUNCTION isConnected(x, y)
        RETURN find(x) == find(y)
    END FUNCTION
END CLASS

Example:
n = 10 (nodes 0-9)
union(0, 1)
union(2, 3)
union(0, 3)
isConnected(1, 2) → true (via 0-1, 0-3, 3-2)
isConnected(1, 4) → false
```

**Complexity:** O(α(n)) ≈ O(1) amortized per operation[9][4]

***

### Intermediate: Number of Islands

**Problem:** Count number of islands (connected 1s) in a grid.[4][9]

```text
FUNCTION numIslands(grid)
    IF grid is empty THEN
        RETURN 0
    END IF
    
    rows = length(grid)
    cols = length(grid[0])
    uf = UnionFind(rows * cols)
    waterCells = 0
    
    // Convert 2D to 1D index
    FUNCTION getIndex(r, c)
        RETURN r * cols + c
    END FUNCTION
    
    directions = [(0,1), (1,0), (0,-1), (-1,0)]
    
    FOR r = 0 TO rows-1 DO
        FOR c = 0 TO cols-1 DO
            IF grid[r][c] == '0' THEN
                waterCells += 1
                CONTINUE
            END IF
            
            // Connect with adjacent land cells
            FOR each (dr, dc) IN directions DO
                nr = r + dr
                nc = c + dc
                IF nr >= 0 AND nr < rows AND 
                   nc >= 0 AND nc < cols AND 
                   grid[nr][nc] == '1' THEN
                    uf.union(getIndex(r, c), getIndex(nr, nc))
                END IF
            END FOR
        END FOR
    END FOR
    
    // Count unique components (excluding water)
    components = SET()
    FOR r = 0 TO rows-1 DO
        FOR c = 0 TO cols-1 DO
            IF grid[r][c] == '1' THEN
                components.add(uf.find(getIndex(r, c)))
            END IF
        END FOR
    END FOR
    
    RETURN size(components)
END FUNCTION

Example:
grid = [['1','1','0','0','0'],
        ['1','1','0','0','0'],
        ['0','0','1','0','0'],
        ['0','0','0','1','1']]
Result: 3 islands
```

**Complexity:** O(m × n × α(m×n))[9][4]

***

### Advanced: Kruskal's Minimum Spanning Tree

**Problem:** Find minimum spanning tree of weighted graph.[4][9]

```text
CLASS Edge
    u, v, weight
END CLASS

FUNCTION kruskalMST(n, edges)
    // Sort edges by weight
    SORT(edges by weight ascending)
    
    uf = UnionFind(n)
    mst = []
    totalCost = 0
    edgesAdded = 0
    
    FOR each edge (u, v, w) IN edges DO
        // If adding edge doesn't create cycle
        IF uf.find(u) != uf.find(v) THEN
            uf.union(u, v)
            mst.append((u, v, w))
            totalCost += w
            edgesAdded += 1
            
            // MST has n-1 edges
            IF edgesAdded == n-1 THEN
                BREAK
            END IF
        END IF
    END FOR
    
    IF edgesAdded < n-1 THEN
        RETURN "Graph not connected"
    END IF
    
    RETURN mst, totalCost
END FUNCTION

Example:
n = 4 (nodes 0-3)
edges = [(0,1,10), (0,2,6), (0,3,5), (1,3,15), (2,3,4)]
Sorted: [(2,3,4), (0,3,5), (0,2,6), (0,1,10), (1,3,15)]

Process:
  Add (2,3,4): connect 2-3, cost=4
  Add (0,3,5): connect 0-3, cost=9
  Add (0,2,6): already connected via 0-3-2, skip
  Add (0,1,10): connect 0-1, cost=19
  3 edges added, done

MST: [(2,3,4), (0,3,5), (0,1,10)], total cost = 19
```

**Complexity:** O(E log E) due to sorting[9][4]

***

### FAANG Problem: Accounts Merge (LeetCode 721)

**Problem:** Merge accounts that share common emails.[4][9]

```text
FUNCTION accountsMerge(accounts)
    uf = UnionFind(length(accounts))
    emailToId = MAP()  // email → account index
    emailToName = MAP()  // email → name
    
    // Build email to account mapping
    FOR i = 0 TO length(accounts)-1 DO
        name = accounts[i][0]
        FOR j = 1 TO length(accounts[i])-1 DO
            email = accounts[i][j]
            emailToName[email] = name
            
            IF email IN emailToId THEN
                // Merge with existing account
                uf.union(i, emailToId[email])
            ELSE
                emailToId[email] = i
            END IF
        END FOR
    END FOR
    
    // Group emails by root account
    components = MAP()  // root → list of emails
    FOR email IN emailToId.keys() DO
        accountId = emailToId[email]
        root = uf.find(accountId)
        IF root NOT IN components THEN
            components[root] = []
        END IF
        components[root].append(email)
    END FOR
    
    // Build result
    result = []
    FOR root IN components.keys() DO
        name = emailToName[components[root][0]]
        emails = SORT(components[root])
        result.append([name] + emails)
    END FOR
    
    RETURN result
END FUNCTION

Example:
accounts = [
  ["John","john@mail.com","john_work@mail.com"],
  ["John","john_smith@mail.com"],
  ["John","john@mail.com","john_new@mail.com"],
  ["Mary","mary@mail.com"]
]

Result: [
  ["John","john@mail.com","john_new@mail.com","john_work@mail.com"],
  ["John","john_smith@mail.com"],
  ["Mary","mary@mail.com"]
]
```

**Complexity:** O(N × K × α(N)) where N = accounts, K = max emails per account[9][4]

***

## 4. Bloom Filter Examples

### Basic: Membership Testing

**Problem:** Check if element might exist with low memory.[10]

```text
CLASS BloomFilter
    bits = BITARRAY(m)
    k = number of hash functions
    hashFunctions = [hash1, hash2, ..., hashk]
    
    FUNCTION add(element)
        FOR i = 0 TO k-1 DO
            index = hashFunctions[i](element) % m
            bits[index] = 1
        END FOR
    END FUNCTION
    
    FUNCTION contains(element)
        FOR i = 0 TO k-1 DO
            index = hashFunctions[i](element) % m
            IF bits[index] == 0 THEN
                RETURN false  // Definitely NOT present
            END IF
        END FOR
        RETURN true  // Probably present
    END FUNCTION
END CLASS

Example:
m = 20 bits, k = 3 hash functions
add("apple")  → set bits at [3, 7, 15]
add("banana") → set bits at [1, 7, 12]
contains("apple") → check [3,7,15] all set → true
contains("orange") → check [2,8,14] → bit 2 not set → false
contains("cherry") → check [1,7,15] all set → true (false positive!)
```

**Complexity:** O(k) for add and contains[10]

---

### Intermediate: URL Deduplication

**Problem:** Check if URL already crawled with minimal memory.[10]

```text
FUNCTION crawlWeb(startUrls)
    visited = BloomFilter(m=10000000, k=5)  // 10M bits
    queue = QUEUE()
    actualVisited = SET()  // For validation
    
    FOR url IN startUrls DO
        queue.enqueue(url)
    END FOR
    
    WHILE NOT queue.isEmpty() DO
        url = queue.dequeue()
        
        // Quick bloom filter check (might have false positives)
        IF visited.contains(url) THEN
            // Double-check with actual set for bloom false positives
            IF url IN actualVisited THEN
                CONTINUE  // Already visited
            END IF
        END IF
        
        // Mark as visited
        visited.add(url)
        actualVisited.add(url)
        
        // Crawl page and extract links
        page = fetchPage(url)
        links = extractLinks(page)
        
        FOR link IN links DO
            IF NOT visited.contains(link) THEN
                queue.enqueue(link)
            END IF
        END FOR
    END FOR
    
    RETURN actualVisited
END FUNCTION
```

**Space Savings:** Bloom filter uses ~1.2 MB vs ~800 MB for hash set (assuming 1M URLs)[10]

***

### Advanced: Distributed Cache

**Problem:** Reduce unnecessary cache lookups across network.[10]

```text
CLASS DistributedCache
    localBloom = BloomFilter(m, k)
    remoteCache = RemoteServer
    
    FUNCTION get(key)
        // Check bloom filter first (local, fast)
        IF NOT localBloom.contains(key) THEN
            RETURN null  // Definitely not in cache
        END IF
        
        // Might be in cache, check remote
        value = remoteCache.get(key)
        RETURN value
    END FUNCTION
    
    FUNCTION put(key, value)
        remoteCache.put(key, value)
        localBloom.add(key)
    END FUNCTION
END CLASS

Benefits:
- Bloom filter: 10 KB memory
- Saves 95% of network calls for non-existent keys
- False positive rate: ~1% (causes unnecessary network call)
- Trade-off: 10 KB memory vs 95% network savings
```

**Complexity:** O(k) for bloom operations, network call only when needed[10]

---

## 5. LRU Cache Examples

### Basic: Simple LRU Cache

**Problem:** Implement cache with O(1) get and put operations.[11]

```text
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

Example:
cache = LRUCache(2)
cache.put(1, 1)  // cache: {1=1}
cache.put(2, 2)  // cache: {1=1, 2=2}
cache.get(1)     // returns 1, cache: {2=2, 1=1}
cache.put(3, 3)  // evicts 2, cache: {1=1, 3=3}
cache.get(2)     // returns -1 (not found)
```

**Complexity:** O(1) for both get and put[11]

***

### Intermediate: LRU with Expiration Time

**Problem:** LRU cache where entries expire after timeout.[11]

```text
CLASS LRUCacheWithExpiry
    capacity
    ttl  // Time-to-live in seconds
    cache = HASHMAP()
    expiryTimes = HASHMAP()  // key → expiry timestamp
    head, tail  // Doubly-linked list
    
    FUNCTION get(key)
        currentTime = getCurrentTime()
        
        IF key NOT IN cache THEN
            RETURN -1
        END IF
        
        // Check if expired
        IF currentTime > expiryTimes[key] THEN
            removeNode(cache[key])
            DELETE cache[key]
            DELETE expiryTimes[key]
            RETURN -1
        END IF
        
        node = cache[key]
        moveToHead(node)
        RETURN node.value
    END FUNCTION
    
    FUNCTION put(key, value)
        currentTime = getCurrentTime()
        
        // Clean up expired entries
        cleanExpired(currentTime)
        
        IF key IN cache THEN
            node = cache[key]
            node.value = value
            expiryTimes[key] = currentTime + ttl
            moveToHead(node)
        ELSE
            IF size(cache) >= capacity THEN
                evictLRU()
            END IF
            
            node = Node(key, value)
            cache[key] = node
            expiryTimes[key] = currentTime + ttl
            addToHead(node)
        END IF
    END FUNCTION
    
    FUNCTION cleanExpired(currentTime)
        // Walk from tail (LRU) removing expired
        node = tail.prev
        WHILE node != head DO
            IF currentTime > expiryTimes[node.key] THEN
                removeNode(node)
                DELETE cache[node.key]
                DELETE expiryTimes[node.key]
            END IF
            node = node.prev
        END WHILE
    END FUNCTION
END CLASS
```

**Complexity:** O(1) amortized with periodic cleanup[11]

---

### Advanced: LFU Cache (Least Frequently Used)

**Problem:** Evict least frequently used item, break ties with LRU.[11]

```text
CLASS LFUCache
    capacity
    minFreq  // Track minimum frequency
    keyToValue = HASHMAP()  // key → value
    keyToFreq = HASHMAP()   // key → frequency
    freqToKeys = HASHMAP()  // frequency → OrderedSet of keys
    
    FUNCTION get(key)
        IF key NOT IN keyToValue THEN
            RETURN -1
        END IF
        
        // Increase frequency
        freq = keyToFreq[key]
        keyToFreq[key] = freq + 1
        
        // Remove from old frequency list
        freqToKeys[freq].remove(key)
        IF freqToKeys[freq].isEmpty() THEN
            DELETE freqToKeys[freq]
            IF minFreq == freq THEN
                minFreq += 1
            END IF
        END IF
        
        // Add to new frequency list
        IF freq+1 NOT IN freqToKeys THEN
            freqToKeys[freq+1] = OrderedSet()
        END IF
        freqToKeys[freq+1].add(key)
        
        RETURN keyToValue[key]
    END FUNCTION
    
    FUNCTION put(key, value)
        IF capacity <= 0 THEN
            RETURN
        END IF
        
        IF key IN keyToValue THEN
            keyToValue[key] = value
            get(key)  // Update frequency
            RETURN
        END IF
        
        // Evict if at capacity
        IF size(keyToValue) >= capacity THEN
            // Remove LFU (and LRU if tie)
            evictKey = freqToKeys[minFreq].first()
            freqToKeys[minFreq].remove(evictKey)
            DELETE keyToValue[evictKey]
            DELETE keyToFreq[evictKey]
        END IF
        
        // Add new key
        keyToValue[key] = value
        keyToFreq[key] = 1
        minFreq = 1
        IF 1 NOT IN freqToKeys THEN
            freqToKeys[1] = OrderedSet()
        END IF
        freqToKeys[1].add(key)
    END FUNCTION
END CLASS

Example:
cache = LFUCache(2)
cache.put(1, 1)  // freq: {1: [1]}
cache.put(2, 2)  // freq: {1: [1,2]}
cache.get(1)     // freq: {1: [2], 2: [1]}, returns 1
cache.put(3, 3)  // evicts 2 (LFU), freq: {1: [3], 2: [1]}
cache.get(2)     // returns -1
cache.get(3)     // freq: {2: [1,3]}, returns 3
```

**Complexity:** O(1) for all operations[11]

---

## 6. Trie Examples

### Basic: Word Dictionary

**Problem:** Insert and search words efficiently.[12]

```text
CLASS TrieNode
    children = ARRAY[26]
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
END CLASS

Example:
trie = Trie()
trie.insert("apple")
trie.search("apple")   → true
trie.search("app")     → false
trie.startsWith("app") → true
trie.insert("app")
trie.search("app")     → true
```

**Complexity:** O(m) for all operations where m = word length[12]

---

### Intermediate: Word Search with Wildcards

**Problem:** Search words with '.' matching any character.[12]

```text
CLASS WordDictionary
    root = TrieNode()
    
    FUNCTION addWord(word)
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
        RETURN searchHelper(word, 0, root)
    END FUNCTION
    
    FUNCTION searchHelper(word, index, node)
        IF node == null THEN
            RETURN false
        END IF
        
        IF index == length(word) THEN
            RETURN node.isEndOfWord
        END IF
        
        char = word[index]
        
        IF char == '.' THEN
            // Try all possible children
            FOR i = 0 TO 25 DO
                IF children[i] != null THEN
                    IF searchHelper(word, index+1, children[i]) THEN
                        RETURN true
                    END IF
                END IF
            END FOR
            RETURN false
        ELSE
            childIndex = char - 'a'
            RETURN searchHelper(word, index+1, children[childIndex])
        END IF
    END FUNCTION
END CLASS

Example:
dict = WordDictionary()
dict.addWord("bad")
dict.addWord("dad")
dict.addWord("mad")
dict.search("pad")  → false
dict.search("bad")  → true
dict.search(".ad")  → true
dict.search("b..")  → true
```

**Complexity:** O(m) for add, O(26^k × m) worst case for search with k wildcards[12]

---

### Advanced: Autocomplete System

**Problem:** Return top k most frequent words matching prefix.[12]

```text
CLASS TrieNode
    children = ARRAY[26]
    isEnd = false
    frequency = 0
    word = ""
END CLASS

CLASS AutocompleteSystem
    root = TrieNode()
    currentNode = root
    currentPrefix = ""
    
    FUNCTION insert(word, frequency)
        node = root
        FOR char IN word DO
            index = char - 'a'
            IF children[index] == null THEN
                children[index] = TrieNode()
            END IF
            node = children[index]
        END FOR
        node.isEnd = true
        node.frequency = frequency
        node.word = word
    END FUNCTION
    
    FUNCTION getSuggestions(char)
        IF char == '#' THEN
            // End of sentence, save input
            insert(currentPrefix, currentNode.frequency + 1)
            currentNode = root
            currentPrefix = ""
            RETURN []
        END IF
        
        currentPrefix += char
        
        IF currentNode == null THEN
            RETURN []
        END IF
        
        index = char - 'a'
        IF currentNode.children[index] == null THEN
            currentNode = null
            RETURN []
        END IF
        
        currentNode = currentNode.children[index]
        
        // DFS to collect all words under current node
        results = []
        collectWords(currentNode, results)
        
        // Sort by frequency (desc), then lexicographically
        SORT(results by frequency desc, then word asc)
        
        // Return top 3
        RETURN results[0:3]
    END FUNCTION
    
    FUNCTION collectWords(node, results)
        IF node == null THEN
            RETURN
        END IF
        
        IF node.isEnd THEN
            results.append((node.word, node.frequency))
        END IF
        
        FOR i = 0 TO 25 DO
            collectWords(node.children[i], results)
        END FOR
    END FUNCTION
END CLASS

Example:
system = AutocompleteSystem()
system.insert("i love you", 5)
system.insert("island", 3)
system.insert("i love leetcode", 2)
system.insert("ironman", 2)

system.getSuggestions('i') → ["i love you", "island", "i love leetcode"]
system.getSuggestions(' ') → ["i love you", "i love leetcode"]
system.getSuggestions('a') → []
system.getSuggestions('#') → []  // Save "i a" with frequency 1
```

**Complexity:** O(m + n log n) where m = prefix length, n = matching words[12]

---

## FAANG Interview Patterns Summary

### Most Common Patterns

| Data Structure | Top Use Cases | Key Problems |
|----------------|---------------|--------------|
| Segment Tree | Range queries, dynamic arrays | Range Sum Query, Skyline Problem |
| Fenwick Tree | Prefix sums, inversions | Count Smaller After Self |
| Union-Find | Graph connectivity, MST | Number of Islands, Accounts Merge |
| Bloom Filter | Deduplication, caching | Web crawlers, distributed systems |
| LRU Cache | Cache systems, eviction | LRU/LFU Cache design |
| Trie | String search, autocomplete | Word Search II, Design Search System |

[1][5][6][3][2][7][8][4][9][10][11][12]

***

**Master these examples to dominate advanced data structure interviews at top tech companies. Practice implementing each from scratch, then apply them to complex problems. The key is understanding when to use each structure and how to optimize for your specific constraints!**[1][6][2][4][9]

[1](https://leetcode.com/problem-list/segment-tree/)
[2](https://leetcode.com/discuss/general-discussion/1128655/introduction-to-segment-tree-a-complete-guide)
[3](https://www.geeksforgeeks.org/dsa/segment-tree-sum-of-given-range/)
[4](https://www.geeksforgeeks.org/dsa/introduction-to-disjoint-set-data-structure-or-union-find-algorithm/)
[5](https://leetcode.com/problems/range-sum-query-mutable/discuss/75741/Segment-Tree-Binary-Indexed-Tree-and-the-simple-way-using-buffer-to-accelerate-in-C++-all-quite-efficient)
[6](https://www.reddit.com/r/leetcode/comments/15wda3q/if_you_havent_studied_segment_trees_so_far_please/)
[7](https://www.geeksforgeeks.org/dsa/binary-indexed-tree-or-fenwick-tree-2/)
[8](https://cp-algorithms.com/data_structures/fenwick.html)
[9](https://www2.hawaii.edu/~nodari/teaching/s18/Notes/Topic-16.html)
[10](https://www.upgrad.com/blog/bloom-filters-in-data-structure/)
[11](https://www.geeksforgeeks.org/system-design/lru-cache-implementation/)
[12](https://www.educative.io/blog/data-structures-tutorial-advanced)
[13](https://www.youtube.com/watch?v=ciHThtTVNto)