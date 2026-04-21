🟢 Hash Table Optimization: Speed, Memory & System Excellence
1. Hash Function Optimization
a. Fast Hash Computation
Problem:
Hash function is called for every operation; must be O(1) with low constants.

Optimization 1: Precompute Hashes

text
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
Benefit: Avoid recomputation on every operation.

Optimization 2: Integer Keys (Direct Mapping)

text
// If key is already integer, skip string hashing
FUNCTION hash(intKey)
    RETURN intKey % size  // Direct modulo
END FUNCTION
Optimization 3: Bit-Level Hash Operations

text
// Use bitwise operations (faster than modulo)
FUNCTION hash(key)
    hash = key * 2654435761  // FNV-1a magic constant
    RETURN (hash >> 16) XOR hash
END FUNCTION

// Modulo via AND (if size = power of 2)
idx = hash AND (size - 1)  // Faster than hash % size
Benefit: 2-3x faster hash computation.

Optimization 4: Specialized Hashes for Common Types

text
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
b. Hash Distribution Quality
Problem:
Poor hash → collisions → degraded performance.

Optimization: Prime Modulo

text
// Bad: size = 2^n (poor distribution for structured data)
idx = hash(key) % (2^16)

// Good: size = prime number (better distribution)
idx = hash(key) % 524287  // Prime
Why Primes: Reduces correlation between hash value bits and index.

Optimization: Avalanche Effect

text
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
Benefit: Better clustering avoidance.

2. Load Factor Optimization
a. Adaptive Resizing
Problem:
Resize too early → wasted space; too late → high collisions.

Optimization: Tuned Thresholds

text
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
Optimization: Exponential Sizing

text
// Avoid frequent resizes by growing exponentially
FUNCTION resize(newSize)
    // Instead of size + 1, use size * 2
    // Amortizes cost over many insertions
    // O(1) amortized cost per insertion
END FUNCTION
Benefit: Less frequent resizing; O(1) amortized cost.

b. Lazy Resizing
text
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
Benefit: No GC spike; smooth performance transition.

3. Collision Resolution Optimization
a. Separate Chaining: Optimize Chains
Problem:
Linked lists have poor cache locality; allocation overhead.

Optimization 1: Use Arrays Instead of Linked Lists

text
// Instead of linked list
bucket[i] = linked_list

// Use dynamic array
bucket[i] = vector  // Contiguous memory; better cache

// Or use inline small list
bucket[i] = SmallVector(capacity=4)  // Few entries most chains
Optimization 2: Hash-within-Bucket

text
CLASS BucketHashTable
    // For each bucket, maintain small hash table
    bucket[i] = {
        miniHash: hash table (size 16)
    }
    
    // Two-level hashing: reduces chain length
    // Chain length avg ≈ α / mini_size
END CLASS
Optimization 3: Move-to-Front

text
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
b. Open Addressing: Optimize Probing
Problem:
Each probe may miss cache; clustering degrades.

Optimization 1: Probe Caching

text
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
Optimization 2: Robin Hood Hashing (Reduce Variance)

text
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
Benefit: Reduces worst-case access time; more uniform.

4. Memory Optimization
a. Use Appropriate Key/Value Types
Problem:
Large keys/values waste memory; hashing cost increases.

Optimization: Key Interning

text
CLASS InternedHashTable
    keyInternPool = SET()
    
    FUNCTION put(key, value)
        // Deduplicate keys; store pointer to interned key
        internedKey = keyInternPool.intern(key)
        bucket[hash(internedKey)] = (internedKey, value)
    END FUNCTION
END CLASS
Benefit: Share key strings across entries; memory savings.

Optimization: Value Compression

text
// Store compressed values; decompress on access
FUNCTION put(key, value)
    compressedValue = compress(value)
    bucket[hash(key)] = (key, compressedValue)
END FUNCTION

FUNCTION get(key)
    entry = lookup(key)
    RETURN decompress(entry.value)
END FUNCTION
Benefit: 5-10x memory reduction for repetitive values.

b. Bit Packing
text
// Store multiple values in single word
CLASS PackedHashEntry
    // Instead of (key, val1, val2, val3)
    // Use (key, packed_word) where packed_word = val1 (5 bits) | val2 (5 bits) | val3 (5 bits)
    
    FUNCTION getValue(fieldIndex)
        RETURN (packedWord >> (fieldIndex * 5)) AND 0x1F
    END FUNCTION
END CLASS
Benefit: 3-4x memory reduction for small values.

c. Sparse Representation
text
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
5. Cache Locality Optimization
a. Bucket Array Alignment
text
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
Benefit: 2-3x speedup from better cache utilization.

b. Data Structure Packing
text
// Pack key, value, hash precomputed in same structure
CLASS PackedEntry
    hash (4 bytes)
    key (8 bytes)
    value (8 bytes)
    // Total: 20 bytes; fits in cache line with others
END CLASS
Benefit: Single cache miss retrieves key, value, hash.

6. Parallel and Concurrent Optimization
a. Lock-Free Hashing
text
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
b. Segmented Locking
text
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
7. Real-World Optimization Patterns
a. Google Dense Hash Map
text
Features:
- Quadratic probing (better distribution)
- Lazy deletion (mark deleted; clean later)
- Precompute hashes (avoid rehashing)
- Single allocation (no linked lists)

Performance: 2-3x faster than standard implementations
b. Facebook Memcached
text
Features:
- Consistent hashing (minimize remapping)
- LRU eviction (O(1) + doubly-linked list)
- Separate chaining (simple, robust)
- Multiple levels (local cache + distributed)

Performance: Sub-millisecond access; handles billions of keys
c. Python Dict (CPython 3.6+)
text
Optimizations:
- Compact representation (array of hashes, keys, values separately)
- Probe caching (remember last probes)
- Unicode key optimization (fast path for strings)
- Hash precomputation (store hash with entry)

Performance: 20-30% faster than older Python 3.5
8. Algorithmic Optimization: Top-K with Hash
Problem:
Find K most frequent elements in stream.

Naive (No Optimization):
text
freqMap = HASHTABLE()
FOR each element:
    freqMap[element] += 1

topK = SORT(freqMap) by frequency
RETURN topK[0:K]

Complexity: O(n log n) for sorting
Optimized (Hash + Min-Heap):
text
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
9. Benchmarking and Profiling
text
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
10. Language-Specific Optimizations
Python
python
# Use dict (built-in; highly optimized)
# NOT: custom implementation
d = {}
d['key'] = 'value'

# Avoid: Large string keys (use hash instead)
# Good: d[hash(longString)] = value
Java
java
// HashMap tuning
HashMap<K, V> map = new HashMap<>(initialCapacity, 0.75f);
// Set capacity > expected size to reduce resizing

// For primitive types, avoid boxing
// IntHashMap from Apache Commons
C++
cpp
// Reserve capacity upfront (avoid reallocations)
std::unordered_map<K, V> map;
map.reserve(expectedSize);

// Use custom hash for better distribution
struct CustomHash {
    size_t operator()(const K& k) const {
        // Optimized hash for your key type
    }
};
11. Common Optimization Pitfalls
Over-optimizing: Profile first; micro-optimize only where needed.

Wrong algorithm: Hash table not always fastest (e.g., small N, use array).

Hash function tuning: Good hash is 80% of performance; choose carefully.

Memory thrashing: Avoid excessive allocations; pre-reserve capacity.

Lock contention: Concurrent access without locks (or careful design).

Cache misses: Poor layout → performance cliffs.

12. Interview Optimization Talking Points
Load factor: Why 0.75; cost of resizing.

Hash quality: Importance of good distribution.

Collision strategies: Trade-offs between chaining and probing.

Cache locality: Why array-based > linked list chains.

Parallelization: Lock-free vs segmented locking.

Space-time trade-off: Precomputed hashes vs recomputation.

13. Summary
Hash table optimization is multi-faceted: fast hash functions, good load factor management, collision strategy selection, cache-aware layout, parallel access patterns. Production systems (Google, Facebook, Python) combine multiple techniques for 10-100x speedups.

Master hash table optimization to build lightning-fast dictionaries, caches, and indices that scale from single-threaded to millions of concurrent operations!