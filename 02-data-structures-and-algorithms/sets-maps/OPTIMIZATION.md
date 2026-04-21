# 🟢 Set & Map Optimization: Speed, Memory & System Excellence

## 1. Hash Function Optimization

### a. Fast Hash Computation

**Problem:**
Hash function called for every operation; must be O(1) with low constants.[1][2]

**Optimization 1: Precompute Hashes**
```text
CLASS OptimizedHashEntry
    key, value
    precomputedHash
    
    FUNCTION __init__(key, value)
        THIS.key = key
        THIS.value = value
        THIS.precomputedHash = hash(key)  // Compute once
    END FUNCTION
END CLASS

CLASS HashTable
    FUNCTION put(key, value)
        entry = OptimizedHashEntry(key, value)
        idx = entry.precomputedHash % size
        // Use precomputed hash
    END FUNCTION
END CLASS
```

**Benefit:** Avoid recomputation on every operation.[1]

***

**Optimization 2: Integer Keys (Direct Mapping)**
```text
// If key is already integer, skip string hashing
FUNCTION hash(intKey)
    RETURN intKey % size  // Direct modulo
END FUNCTION
```

***

**Optimization 3: Bit-Level Hash Operations**
```text
// Use bitwise operations (faster than modulo)
FUNCTION hash(key)
    hash = key * 2654435761  // FNV-1a magic constant
    RETURN (hash >> 16) XOR hash
END FUNCTION

// Modulo via AND (if size = power of 2)
idx = hash AND (size - 1)  // Faster than hash % size
```

**Benefit:** 2-3x faster hash computation.[1]

***

**Optimization 4: Specialized Hashes for Common Types**
```text
// Cache-friendly for strings
FUNCTION fastStringHash(s)
    hash = 5381
    FOR char IN s DO
        hash = hash * 33 XOR char  // FNV-like
    END FOR
    RETURN hash
END FUNCTION

// For short strings, use inline computation
#pragma inline
FUNCTION fastShortStringHash(s)
    // Unroll loop; specialized for len ≤ 16
    // Direct memory access; no function calls
END FUNCTION
```

***

### b. Hash Distribution Quality

**Problem:**
Poor hash → collisions → degraded performance.[3][1]

**Optimization: Prime Modulo**
```text
// Bad: size = 2^n (poor distribution for structured data)
idx = hash(key) % (2^16)

// Good: size = prime number (better distribution)
idx = hash(key) % 524287  // Prime
```

**Why Primes:** Reduces correlation between hash value bits and index.[3][1]

***

**Optimization: Avalanche Effect**
```text
// Bad: Small input change → small hash change
hash = key % size

// Good: Avalanche (small change → large hash change)
FUNCTION avalanching(x)
    x = (x XOR 61) XOR (x >> 16)
    x = x + (x << 3)
    x = x XOR (x >> 4)
    x = x * 0x27d4eb2d
    x = x XOR (x >> 15)
    RETURN x
END FUNCTION
```

**Benefit:** Better clustering avoidance.[1]

***

## 2. Load Factor Optimization

### a. Adaptive Resizing

**Problem:**
Resize too early → wasted space; too late → high collisions.[3][1]

**Optimization: Tuned Thresholds**
```text
CLASS OptimizedHashTable
    loadFactorUpperBound = 0.75
    loadFactorLowerBound = 0.25
    
    FUNCTION put(key, value)
        // ... insert ...
        IF (count + 1) / size > loadFactorUpperBound THEN
            resize(size * 2)
        END IF
    END FUNCTION
    
    FUNCTION remove(key)
        // ... remove ...
        IF count / size < loadFactorLowerBound AND size > INITIAL_SIZE THEN
            resize(size / 2)  // Shrink to save memory
        END IF
    END FUNCTION
END CLASS
```

***

**Optimization: Exponential Sizing**
```text
// Avoid frequent resizes by growing exponentially
FUNCTION resize(newSize)
    // Instead of size + 1, use size * 2
    // Amortizes cost over many insertions
    // O(1) amortized cost per insertion
END FUNCTION
```

**Benefit:** Less frequent resizing; O(1) amortized cost.[1]

---

### b. Lazy Resizing

```text
CLASS LazyHashTable
    oldBuckets = null
    newBuckets = null
    rehashIndex = 0
    resizingInProgress = false
    
    FUNCTION put(key, value)
        IF resizingInProgress THEN
            rehashOneBucket()  // Incremental rehash
        END IF
        // ... insert into newBuckets ...
    END FUNCTION
    
    FUNCTION rehashOneBucket()
        // Rehash one bucket per operation
        // Spread cost over time; avoid GC spike
        // Gradual transition from old to new
    END FUNCTION
END CLASS
```

**Benefit:** No GC spike; smooth performance transition.[1]

***

## 3. Collision Resolution Optimization

### a. Separate Chaining: Optimize Chains

**Problem:**
Linked lists have poor cache locality; allocation overhead.[2][1]

**Optimization 1: Use Arrays Instead of Linked Lists**
```text
// Instead of linked list
bucket[i] = linked_list

// Use dynamic array
bucket[i] = vector  // Contiguous memory; better cache

// Or use inline small list
bucket[i] = SmallVector(capacity=4)  // Few entries most chains
```

***

**Optimization 2: Hash-within-Bucket**
```text
CLASS BucketHashTable
    // For each bucket, maintain small hash table
    bucket[i] = {
        miniHash: hash table (size 16)
    }
    
    // Two-level hashing: reduces chain length
    // Chain length avg ≈ α / mini_size
END CLASS
```

***

**Optimization 3: Move-to-Front**
```text
FUNCTION get(key)
    idx = hash(key) % size
    chain = bucket[idx]
    
    // Find entry
    entry = chain.find(key)
    
    // Move found entry to front
    chain.moveToFront(entry)
    
    RETURN entry.value
END FUNCTION

// Benefit: Frequently accessed entries at chain head
// Average search distance reduced
```

***

### b. Open Addressing: Optimize Probing

**Problem:**
Each probe may miss cache; clustering degrades.[2][1]

**Optimization 1: Probe Caching**
```text
CLASS CachedProbeHashTable
    lastProbeSequence = []  // Cache last probes
    
    FUNCTION get(key)
        idx = hash(key) % size
        
        // Check cached sequence first
        FOR probeIdx IN lastProbeSequence DO
            IF bucket[probeIdx].key == key THEN
                RETURN bucket[probeIdx].value
            END IF
        END FOR
        
        // Standard probing if not in cache
        // Update cache for next time
    END FUNCTION
END CLASS
```

***

**Optimization 2: Robin Hood Hashing (Reduce Variance)**
```text
// Track how far each entry is from home position
// On collision: steal from richer entries

FUNCTION put(key, value)
    idx = hash(key) % size
    distance = 0
    
    WHILE true DO
        probeIdx = (idx + distance) % size
        
        IF bucket[probeIdx] == null THEN
            bucket[probeIdx] = (key, value, distance)
            RETURN
        END IF
        
        existingDistance = bucket[probeIdx].distance
        IF distance > existingDistance THEN
            SWAP (key, value, distance) with bucket[probeIdx]
        END IF
        
        distance += 1
    END WHILE
END FUNCTION
```

**Benefit:** Reduces worst-case access time; more uniform.[1]

---

## 4. Memory Optimization

### a. Use Appropriate Key/Value Types

**Problem:**
Large keys/values waste memory; hashing cost increases.[2][1]

**Optimization: Key Interning**
```text
CLASS InternedHashTable
    keyInternPool = SET()
    
    FUNCTION put(key, value)
        // Deduplicate keys; store pointer to interned key
        internedKey = keyInternPool.intern(key)
        bucket[hash(internedKey)] = (internedKey, value)
    END FUNCTION
END CLASS
```

**Benefit:** Share key strings across entries; memory savings.[1]

***

**Optimization: Value Compression**
```text
// Store compressed values; decompress on access
FUNCTION put(key, value)
    compressedValue = compress(value)
    bucket[hash(key)] = (key, compressedValue)
END FUNCTION

FUNCTION get(key)
    entry = lookup(key)
    RETURN decompress(entry.value)
END FUNCTION
```

**Benefit:** 5-10x memory reduction for repetitive values.[1]

***

### b. Bit Packing

```text
// Store multiple values in single word
CLASS PackedHashEntry
    // Instead of (key, val1, val2, val3)
    // Use (key, packed_word) where packed_word = val1 (5 bits) | val2 (5 bits) | val3 (5 bits)
    
    FUNCTION getValue(fieldIndex)
        RETURN (packedWord >> (fieldIndex * 5)) AND 0x1F
    END FUNCTION
END CLASS
```

**Benefit:** 3-4x memory reduction for small values.[1]

***

### c. Sparse Representation

```text
// Don't store hash table; only store active entries
CLASS SparseHashTable
    entries = LIST()  // Only non-null entries
    
    FUNCTION put(key, value)
        FOR entry IN entries DO
            IF entry.key == key THEN
                entry.value = value
                RETURN
            END IF
        END FOR
        entries.append((key, value))
    END FUNCTION
    
    FUNCTION get(key)
        FOR entry IN entries DO
            IF entry.key == key THEN
                RETURN entry.value
            END IF
        END FOR
        RETURN null
    END FUNCTION
END CLASS

// Trade-off: O(n) lookup but O(n) space (no empty slots)
// Good for very sparse tables (< 1% fill)
```

***

## 5. Cache Locality Optimization

### a. Bucket Array Alignment

```text
// Allocate buckets to fit in cache line
CLASS CacheAlignedHashTable
    bucketSize = 64  // Cache line size
    buckets = ARRAY aligned to 64-byte boundary
    
    FUNCTION put(key, value)
        idx = hash(key) % size
        // bucket[idx] aligned to cache line start
        // Multiple entries fit in same cache line
    END FUNCTION
END CLASS
```

**Benefit:** 2-3x speedup from better cache utilization.[4][2][1]

**Performance Impact:**
"Main memory reference is 20x times slower than L2 cache access and 200x times slower than L1 cache access. In practice, memory access latency is around 100ns".[2]

***

### b. Data Structure Packing

```text
// Pack key, value, hash precomputed in same structure
CLASS PackedEntry
    hash (4 bytes)
    key (8 bytes)
    value (8 bytes)
    // Total: 20 bytes; fits in cache line with others
END CLASS
```

**Benefit:** Single cache miss retrieves key, value, hash.[4][1]

***

### c. Loop Transformations

**Loop Tiling (Blocking):**
```text
// Original matrix multiplication
FOR i = 0 TO N-1 DO
    FOR j = 0 TO N-1 DO
        FOR k = 0 TO N-1 DO
            C[i][j] += A[i][k] * B[k][j]

// Tiled matrix multiplication
TILE_SIZE = 32
FOR i = 0 TO N-1 STEP TILE_SIZE DO
    FOR j = 0 TO N-1 STEP TILE_SIZE DO
        FOR k = 0 TO N-1 STEP TILE_SIZE DO
            FOR ii = i TO min(i + TILE_SIZE, N) - 1 DO
                FOR jj = j TO min(j + TILE_SIZE, N) - 1 DO
                    FOR kk = k TO min(k + TILE_SIZE, N) - 1 DO
                        C[ii][jj] += A[ii][kk] * B[kk][jj]
```

**Benefit:** Improved cache utilization by operating on smaller chunks.[5][4]

***

## 6. Parallel and Concurrent Optimization

### a. Lock-Free Hashing

```text
// Use compare-and-swap (CAS) for thread-safe updates
CLASS LockFreeHashTable
    buckets = ATOMIC_ARRAY
    
    FUNCTION put(key, value)
        idx = hash(key) % size
        
        REPEAT
            oldValue = buckets[idx]
            IF CAS(buckets[idx], oldValue, newValue) THEN
                RETURN
            END IF
        UNTIL success
    END FUNCTION
END CLASS

// Benefit: No locks; scales to many threads
// Trade-off: More complex; retry logic
```

***

### b. Segmented Locking

```text
// Multiple locks; one per segment (not per entry)
CLASS SegmentedHashTable
    numSegments = 16
    locks = ARRAY[numSegments]
    
    FUNCTION put(key, value)
        idx = hash(key) % size
        segmentIdx = idx / (size / numSegments)
        
        LOCK locks[segmentIdx]
        bucket[idx] = (key, value)
        UNLOCK locks[segmentIdx]
    END FUNCTION
END CLASS

// Benefit: Parallelism; less contention than single lock
```

***

## 7. Set-Specific Optimizations

### a. Bit Vector Set (Enum Set)

```text
CLASS BitVectorSet
    bits = BITARRAY(maxValue)
    
    FUNCTION add(element)
        bits[element] = 1
    END FUNCTION
    
    FUNCTION remove(element)
        bits[element] = 0
    END FUNCTION
    
    FUNCTION contains(element)
        RETURN bits[element] == 1
    END FUNCTION
    
    FUNCTION union(otherSet)
        result = BitVectorSet()
        result.bits = THIS.bits OR otherSet.bits
        RETURN result
    END FUNCTION
    
    FUNCTION intersection(otherSet)
        result = BitVectorSet()
        result.bits = THIS.bits AND otherSet.bits
        RETURN result
    END FUNCTION
END CLASS
```

**Complexity:**
- Operations: O(1)
- Set operations: O(n/64) using bitwise operations
- Space: O(maxValue) bits

**When to Use:**
- Small integer domains (0-1000)
- Enum types
- Extremely fast set operations

***

### b. Bloom Filter Optimization

```text
CLASS OptimizedBloomFilter
    bits = BITARRAY(size)
    numHashFunctions = k
    
    // Optimization: Single hash function with multiple outputs
    FUNCTION add(element)
        h1 = hash1(element)
        h2 = hash2(element)
        
        FOR i = 0 TO k - 1 DO
            idx = (h1 + i * h2) % size  // Double hashing
            bits[idx] = 1
        END FOR
    END FUNCTION
END CLASS
```

**Benefit:** One hash computation generates k indices; 3-5x faster.[1]

***

## 8. Algorithmic Optimization Patterns

### a. Top-K with Hash + Heap

**Problem:**
Find K most frequent elements in stream.[1]

**Naive (No Optimization):**
```text
freqMap = HASHTABLE()
FOR each element:
    freqMap[element] += 1

topK = SORT(freqMap) by frequency
RETURN topK[0:K]

Complexity: O(n log n) for sorting
```

**Optimized (Hash + Min-Heap):**
```text
freqMap = HASHTABLE()  // element → frequency
minHeap = MINHEAP()    // min-heap of K elements by frequency

FOR each element:
    freqMap[element] += 1
    freq = freqMap[element]
    
    IF minHeap.size() < K:
        minHeap.push((freq, element))
    ELSE IF freq > minHeap.peek().freq:
        minHeap.pop()
        minHeap.push((freq, element))

Complexity: O(n log K) instead of O(n log n)
For K << n: 10-100x faster
```

***

### b. Frequency-Based Cache Eviction (LFU)

```text
CLASS LFUCache
    cache = HashMap()  // key → value
    freqMap = HashMap()  // frequency → list of keys
    keyFreq = HashMap()  // key → current frequency
    minFreq = 0
    
    FUNCTION get(key)
        IF key NOT IN cache THEN
            RETURN -1
        END IF
        
        // Increment frequency
        freq = keyFreq[key]
        freqMap[freq].remove(key)
        keyFreq[key] = freq + 1
        freqMap[freq + 1].add(key)
        
        // Update minFreq if needed
        IF freqMap[minFreq].isEmpty() THEN
            minFreq = freq + 1
        END IF
        
        RETURN cache[key]
    END FUNCTION
    
    FUNCTION put(key, value)
        IF key IN cache THEN
            cache[key] = value
            get(key)  // Update frequency
            RETURN
        END IF
        
        IF cache.size() >= capacity THEN
            // Evict least frequently used
            evictKey = freqMap[minFreq].removeFirst()
            cache.remove(evictKey)
            keyFreq.remove(evictKey)
        END IF
        
        cache[key] = value
        keyFreq[key] = 1
        freqMap[1].add(key)
        minFreq = 1
    END FUNCTION
END CLASS
```

**Complexity:** O(1) for all operations.[1]

***

## 9. Real-World Optimization Patterns

### a. Google Dense Hash Map

```text
Features:
- Quadratic probing (better distribution)
- Lazy deletion (mark deleted; clean later)
- Precompute hashes (avoid rehashing)
- Single allocation (no linked lists)

Performance: 2-3x faster than standard implementations
```

***

### b. Facebook Memcached

```text
Features:
- Consistent hashing (minimize remapping)
- LRU eviction (O(1) + doubly-linked list)
- Separate chaining (simple, robust)
- Multiple levels (local cache + distributed)

Performance: Sub-millisecond access; handles billions of keys
```

***

### c. Python Dict (CPython 3.6+)

```text
Optimizations:
- Compact representation (array of hashes, keys, values separately)
- Probe caching (remember last probes)
- Unicode key optimization (fast path for strings)
- Hash precomputation (store hash with entry)

Performance: 20-30% faster than older Python 3.5
```

***

### d. ClickHouse Hash Tables

**Specialized for database operations**:[2]

**String Hash Tables:**
```text
4 specialized tables for different string lengths:
- 0-8 bytes
- 9-16 bytes
- 17-24 bytes
- >24 bytes
```

**Two-Level Hash Tables:**
```text
256 hash tables for multi-threaded GROUP BY
"When we need to join tables, we use minimal synchronization"
```

**Benefit:** Optimized for specific data types and workloads.[2]

***

## 10. Dynamic Hashing Techniques

### a. Extendible Hashing

```text
CLASS ExtendibleHashTable
    directory = ARRAY[2^depth]
    depth = 0
    
    FUNCTION put(key, value)
        idx = hash(key) & ((1 << depth) - 1)
        bucket = directory[idx]
        
        IF bucket.isFull() THEN
            splitBucket(bucket)
            put(key, value)  // Retry after split
        ELSE
            bucket.add(key, value)
        END IF
    END FUNCTION
    
    FUNCTION splitBucket(bucket)
        // Split bucket; update directory
        // Increase depth if needed
    END FUNCTION
END CLASS
```

**Benefit:** Dynamic expansion without excessive overhead.[3]

***

### b. Linear Hashing

```text
CLASS LinearHashTable
    buckets = ARRAY
    splitPointer = 0
    
    FUNCTION put(key, value)
        insert(key, value)
        
        IF loadFactor() > threshold THEN
            splitBucket(splitPointer)
            splitPointer = (splitPointer + 1) % buckets.length
        END IF
    END FUNCTION
    
    FUNCTION splitBucket(idx)
        // Incrementally add new buckets as dataset grows
    END FUNCTION
END CLASS
```

**Benefit:** Straightforward resizing approach; incremental growth.[3]

***

## 11. Benchmarking and Profiling

```text
FUNCTION profileHashTable()
    startTime = NOW()
    startMemory = MEMORY_USAGE()
    cacheMisses = PERF_COUNTER_START()
    
    FOR i = 1 TO numOperations DO
        hashTable.put(key[i], value[i])
    END FOR
    
    endTime = NOW()
    endMemory = MEMORY_USAGE()
    cacheMisses = PERF_COUNTER_STOP()
    
    LOG("Time: ", endTime - startTime, "ms")
    LOG("Memory: ", endMemory - startMemory, "MB")
    LOG("Cache misses: ", cacheMisses)
    LOG("Operations/sec: ", numOperations / (endTime - startTime))
    
    // Identify bottleneck
    IF cacheMisses > threshold THEN
        RECOMMEND "Optimize cache locality"
    ELSE IF timePerOp > threshold THEN
        RECOMMEND "Optimize hash function"
    END IF
END FUNCTION
```

**Performance Factors**:[2]
1. **Fits in CPU cache:** Focus on arithmetic operations, hash computation
2. **Doesn't fit in cache:** Focus on reducing random memory accesses

***

## 12. Language-Specific Optimizations

### Python
```python
# Use dict (built-in; highly optimized)
# NOT: custom implementation
d = {}
d['key'] = 'value'

# Avoid: Large string keys (use hash instead)
# Good: d[hash(longString)] = value
```

***

### Java
```java
// HashMap tuning
HashMap<K, V> map = new HashMap<>(initialCapacity, 0.75f);
// Set capacity > expected size to reduce resizing

// For primitive types, avoid boxing
// IntHashMap from Apache Commons
```

***

### C++
```cpp
// Reserve capacity upfront (avoid reallocations)
std::unordered_map<K, V> map;
map.reserve(expectedSize);

// Use custom hash for better distribution
struct CustomHash {
    size_t operator()(const K& k) const {
        // Optimized hash for your key type
    }
};
```

***

### JavaScript
```javascript
// Use Map (native; optimized)
const map = new Map();
map.set('key', 'value');

// WeakMap for automatic garbage collection
const weakMap = new WeakMap();
```

***

## 13. Load Factor vs Performance Trade-offs

| Load Factor | Pros | Cons | Best For |
|-------------|------|------|----------|
| 0.5 | Excellent performance; minimal collisions | 2x memory overhead | Performance-critical systems [2] |
| 0.75 | Balanced speed/memory | Standard choice | General-purpose applications [1] |
| 0.875+ | Memory-efficient | Slower lookups; more collisions | Memory-constrained systems [2] |

**Recommendation:**
"Linear probing open addressing hash tables can work well only with a load factor near 0.5... there are also hash table implementations that can work well with load factor like 0.875 or even higher, but in practice, they will provide slower lookup latency".[2]

---

## 14. Common Optimization Pitfalls

1. **Over-optimizing:** Profile first; micro-optimize only where needed[1]
2. **Wrong algorithm:** Hash table not always fastest (e.g., small N, use array)[1]
3. **Hash function tuning:** Good hash is 80% of performance; choose carefully[1]
4. **Memory thrashing:** Avoid excessive allocations; pre-reserve capacity[1]
5. **Lock contention:** Concurrent access without locks (or careful design)[1]
6. **Cache misses:** Poor layout → performance cliffs[2][1]

***

## 15. Interview Optimization Talking Points

- **Load factor:** Why 0.75; cost of resizing[1]
- **Hash quality:** Importance of good distribution[1]
- **Collision strategies:** Trade-offs between chaining and probing[1]
- **Cache locality:** Why array-based > linked list chains[2][1]
- **Parallelization:** Lock-free vs segmented locking[1]
- **Space-time trade-off:** Precomputed hashes vs recomputation[1]
- **Dynamic resizing:** Exponential growth vs incremental[3]

***

## 16. Summary: Optimization Hierarchy

**Level 1: Algorithmic (Highest Impact)**
- Choose right data structure (hash set vs tree set)
- Optimize collision resolution strategy
- Tune load factor appropriately

**Level 2: Implementation**
- Fast hash functions (bitwise ops, precomputation)
- Cache-friendly layout (alignment, packing)
- Memory efficiency (compression, bit packing)

**Level 3: System-Level**
- Concurrent access patterns (lock-free, segmented)
- NUMA awareness
- Prefetching and vectorization (SIMD)

**Level 4: Hardware-Specific**
- CPU cache tuning
- Memory bandwidth optimization
- Branch prediction optimization

***

**Hash table optimization is multi-faceted: fast hash functions, good load factor management, collision strategy selection, cache-aware layout, parallel access patterns. Production systems (Google, Facebook, Python) combine multiple techniques for 10-100x speedups. Master set and map optimization to build lightning-fast dictionaries, caches, and indices that scale from single-threaded to millions of concurrent operations!**[5][4][3][2][1]

[1](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/87948039/b859fc17-ad32-4ee4-9a1b-d46a9f73af67/OPTIMIZATION.md)
[2](https://maksimkita.com/blog/hash_tables.html)
[3](https://chat2db.ai/resources/blog/optimize-hashing-in-dbms)
[4](https://www.sciencedirect.com/topics/computer-science/cache-optimization)
[5](https://algocademy.com/blog/understanding-cache-and-memory-optimization-boosting-your-codes-performance/)
[6](https://www.ronkot.com/google-maps-optimization/)
[7](https://arxiv.org/abs/2509.07836)
[8](https://developers.google.com/maps/optimization-guide)
[9](https://seoquartz.com/blog/google-maps-optimization-voice-search-2025/)
[10](https://www.onthemap.com/blog/how-to-optimize-google-my-business/)
[11](https://digicobweb.com/google-business-profile-guide-2025/)