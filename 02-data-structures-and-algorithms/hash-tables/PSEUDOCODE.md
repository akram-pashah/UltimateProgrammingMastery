🟢 Pseudocode: Hash Tables – Core Operations to Advanced Algorithms
1. Basic Hash Function
text
FUNCTION simpleHash(key, tableSize)
    // Sum of character ASCII values modulo table size
    hash = 0
    FOR char IN key DO
        hash += ASCII(char)
    END FOR
    RETURN hash % tableSize
END FUNCTION

FUNCTION polynomialRollingHash(key, tableSize)
    // Polynomial rolling hash: O(|key|)
    hash = 0
    base = 31
    power = 1
    FOR char IN key DO
        hash += (ASCII(char) * power) % tableSize
        power = (power * base) % tableSize
    END FOR
    RETURN hash % tableSize
END FUNCTION

FUNCTION djb2Hash(key, tableSize)
    // DJB2 hash function (good distribution)
    hash = 5381
    FOR char IN key DO
        hash = ((hash << 5) + hash) + ASCII(char)
    END FOR
    RETURN ABS(hash) % tableSize
END FUNCTION
2. Hash Table with Separate Chaining
text
CLASS HashTableChaining
    buckets = ARRAY[size]
    size = 16
    count = 0
    
    FUNCTION hash(key)
        // Simple hash function
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
        
        // Check load factor; resize if needed
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
            RETURN
        END IF
        
        FOR i = 0 TO len(buckets[idx]) - 1 DO
            IF buckets[idx][i].key == key THEN
                buckets[idx].removeAt(i)
                count -= 1
                RETURN
            END IF
        END FOR
    END FUNCTION
    
    FUNCTION contains(key)
        RETURN get(key) != null
    END FUNCTION
    
    FUNCTION isEmpty()
        RETURN count == 0
    END FUNCTION
    
    FUNCTION size()
        RETURN count
    END FUNCTION
END CLASS
Complexity: O(1) average insert/search/delete, O(n) worst-case.

3. Hash Table with Open Addressing (Linear Probing)
text
CLASS HashTableOpenAddressing
    buckets = ARRAY[size]
    size = 16
    count = 0
    TOMBSTONE = "DELETED"
    
    FUNCTION hash(key)
        hash = 0
        FOR char IN key DO
            hash += ASCII(char)
        END FOR
        RETURN hash % size
    END FUNCTION
    
    FUNCTION put(key, value)
        idx = hash(key)
        probes = 0
        
        WHILE probes < size DO
            IF buckets[idx] == null OR buckets[idx] == TOMBSTONE THEN
                buckets[idx] = (key, value)
                count += 1
                IF count / size > 0.75 THEN
                    resize()
                END IF
                RETURN
            ELSE IF buckets[idx].key == key THEN
                buckets[idx].value = value  // Update existing
                RETURN
            END IF
            
            idx = (idx + 1) % size  // Linear probing
            probes += 1
        END WHILE
        
        ERROR "Hash table full"
    END FUNCTION
    
    FUNCTION get(key)
        idx = hash(key)
        probes = 0
        
        WHILE probes < size AND buckets[idx] != null DO
            IF buckets[idx] != TOMBSTONE AND buckets[idx].key == key THEN
                RETURN buckets[idx].value
            END IF
            
            idx = (idx + 1) % size
            probes += 1
        END WHILE
        
        RETURN null
    END FUNCTION
    
    FUNCTION remove(key)
        idx = hash(key)
        probes = 0
        
        WHILE probes < size AND buckets[idx] != null DO
            IF buckets[idx] != TOMBSTONE AND buckets[idx].key == key THEN
                buckets[idx] = TOMBSTONE
                count -= 1
                RETURN
            END IF
            
            idx = (idx + 1) % size
            probes += 1
        END WHILE
    END FUNCTION
END CLASS
Advantage: Better cache locality than separate chaining.
Disadvantage: Clustering; tombstone overhead.

4. Hash Table with Quadratic Probing
text
CLASS HashTableQuadraticProbing
    buckets = ARRAY[size]
    size = 16
    count = 0
    
    FUNCTION put(key, value)
        idx = hash(key)
        i = 0
        
        WHILE i < size DO
            probeIdx = (idx + i*i) % size
            
            IF buckets[probeIdx] == null THEN
                buckets[probeIdx] = (key, value)
                count += 1
                RETURN
            ELSE IF buckets[probeIdx].key == key THEN
                buckets[probeIdx].value = value
                RETURN
            END IF
            
            i += 1
        END WHILE
        
        ERROR "Hash table full"
    END FUNCTION
    
    FUNCTION get(key)
        idx = hash(key)
        i = 0
        
        WHILE i < size DO
            probeIdx = (idx + i*i) % size
            
            IF buckets[probeIdx] == null THEN
                RETURN null
            ELSE IF buckets[probeIdx].key == key THEN
                RETURN buckets[probeIdx].value
            END IF
            
            i += 1
        END WHILE
        
        RETURN null
    END FUNCTION
END CLASS
Benefit: Reduces clustering vs linear probing.

5. Hash Table with Double Hashing
text
CLASS HashTableDoubleHashing
    buckets = ARRAY[size]
    size = 16
    count = 0
    
    FUNCTION hash1(key)
        hash = 0
        FOR char IN key DO
            hash += ASCII(char)
        END FOR
        RETURN hash % size
    END FUNCTION
    
    FUNCTION hash2(key)
        // Second hash function; should be coprime to size
        hash = 0
        FOR char IN key DO
            hash = hash * 31 + ASCII(char)
        END FOR
        RETURN 1 + (hash % (size - 1))
    END FUNCTION
    
    FUNCTION put(key, value)
        idx = hash1(key)
        step = hash2(key)
        i = 0
        
        WHILE i < size DO
            probeIdx = (idx + i * step) % size
            
            IF buckets[probeIdx] == null THEN
                buckets[probeIdx] = (key, value)
                count += 1
                RETURN
            ELSE IF buckets[probeIdx].key == key THEN
                buckets[probeIdx].value = value
                RETURN
            END IF
            
            i += 1
        END WHILE
        
        ERROR "Hash table full"
    END FUNCTION
    
    FUNCTION get(key)
        idx = hash1(key)
        step = hash2(key)
        i = 0
        
        WHILE i < size DO
            probeIdx = (idx + i * step) % size
            
            IF buckets[probeIdx] == null THEN
                RETURN null
            ELSE IF buckets[probeIdx].key == key THEN
                RETURN buckets[probeIdx].value
            END IF
            
            i += 1
        END WHILE
        
        RETURN null
    END FUNCTION
END CLASS
Benefit: Best distribution; fewer collisions than linear/quadratic.

6. Resizing (Rehashing)
text
FUNCTION resize()
    oldBuckets = buckets
    oldSize = size
    
    // Double the size (or pick next prime)
    size = oldSize * 2
    buckets = ARRAY[size]
    count = 0
    
    // Rehash all entries
    FOR bucket IN oldBuckets DO
        IF bucket != null THEN
            FOR pair IN bucket DO
                put(pair.key, pair.value)
            END FOR
        END IF
    END FOR
END FUNCTION
Complexity: O(n) per resize (amortized O(1) per insertion).

7. LRU Cache with Hash Table
text
CLASS LRUCache
    capacity = 0
    cache = HASHTABLE()  // key -> value
    order = DoublyLinkedList()  // Track access order
    keyToNode = HASHTABLE()  // key -> node
    
    FUNCTION __init__(capacity)
        THIS.capacity = capacity
    END FUNCTION
    
    FUNCTION get(key)
        IF NOT cache.contains(key) THEN
            RETURN -1
        END IF
        
        // Move to front (most recent)
        node = keyToNode[key]
        order.moveToFront(node)
        
        RETURN cache[key]
    END FUNCTION
    
    FUNCTION put(key, value)
        IF cache.contains(key) THEN
            // Update value and move to front
            cache[key] = value
            node = keyToNode[key]
            order.moveToFront(node)
        ELSE
            // Check capacity
            IF cache.size() >= capacity THEN
                // Remove least recently used (tail)
                lruKey = order.removeTail()
                cache.remove(lruKey)
                keyToNode.remove(lruKey)
            END IF
            
            // Add new entry to front
            cache[key] = value
            node = order.addFront(key)
            keyToNode[key] = node
        END IF
    END FUNCTION
END CLASS
Complexity: O(1) for all operations.

8. Bloom Filter
text
CLASS BloomFilter
    bits = BITARRAY(size)
    size = 0
    numHashFunctions = 0
    
    FUNCTION __init__(size, numHashFunctions)
        THIS.size = size
        THIS.numHashFunctions = numHashFunctions
        bits = BITARRAY(size, initialized to 0)
    END FUNCTION
    
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
                RETURN false  // Definitely not in set
            END IF
        END FOR
        RETURN true  // Probably in set (false positive possible)
    END FUNCTION
    
    FUNCTION hash(element, hashNum)
        // Generate different hash for each hashNum
        hash = element.hashCode()
        FOR i = 0 TO hashNum DO
            hash = hash * 31 + hashNum
        END FOR
        RETURN ABS(hash)
    END FUNCTION
END CLASS
Space: O(m) bits (much smaller than hash table).
False positive rate: Tunable by number of hash functions.

9. Consistent Hashing (Distributed Systems)
text
CLASS ConsistentHash
    ring = HASHTABLE()  // hash(node) -> node
    sortedKeys = []
    numReplicas = 3
    
    FUNCTION addNode(node)
        FOR i = 0 TO numReplicas - 1 DO
            hash = hashFunction(node + "_" + i.toString())
            ring[hash] = node
            sortedKeys.append(hash)
        END FOR
        SORT(sortedKeys)
    END FUNCTION
    
    FUNCTION removeNode(node)
        FOR i = 0 TO numReplicas - 1 DO
            hash = hashFunction(node + "_" + i.toString())
            ring.remove(hash)
            sortedKeys.remove(hash)
        END FOR
    END FUNCTION
    
    FUNCTION getNode(key)
        hash = hashFunction(key)
        
        // Binary search for smallest hash >= key hash
        idx = binarySearchLeftmost(sortedKeys, hash)
        
        IF idx >= len(sortedKeys) THEN
            idx = 0  // Wrap around to start
        END IF
        
        RETURN ring[sortedKeys[idx]]
    END FUNCTION
    
    FUNCTION binarySearchLeftmost(arr, target)
        left = 0
        right = len(arr)
        
        WHILE left < right DO
            mid = (left + right) / 2
            IF arr[mid] < target THEN
                left = mid + 1
            ELSE
                right = mid
            END IF
        END WHILE
        
        RETURN left
    END FUNCTION
END CLASS
Benefit: O(k/n) keys affected on node add/remove (k = total, n = nodes).

10. Two-Sum Using Hash Table
text
FUNCTION twoSum(arr, target)
    // Find two numbers that sum to target
    numMap = HASHTABLE()
    
    FOR i = 0 TO len(arr) - 1 DO
        complement = target - arr[i]
        
        IF numMap.contains(complement) THEN
            RETURN [numMap[complement], i]
        END IF
        
        numMap[arr[i]] = i
    END FOR
    
    RETURN []  // Not found
END FUNCTION
Complexity: O(n) time, O(n) space.

11. Frequency Counter (Top-K)
text
FUNCTION topKFrequent(arr, k)
    // Count frequencies
    freqMap = HASHTABLE()
    FOR num IN arr DO
        freqMap[num] = freqMap.get(num, 0) + 1
    END FOR
    
    // Use min-heap for top-K
    minHeap = MINHEAP()
    FOR num IN freqMap.keys() DO
        freq = freqMap[num]
        minHeap.push((freq, num))
        IF minHeap.size() > k THEN
            minHeap.pop()
        END IF
    END FOR
    
    result = []
    WHILE NOT minHeap.isEmpty() DO
        freq, num = minHeap.pop()
        result.append(num)
    END WHILE
    
    RETURN result
END FUNCTION
Complexity: O(n log k).

These pseudocode implementations provide the foundation for implementing hash tables in any programming language and solving hash-based problems efficiently!

