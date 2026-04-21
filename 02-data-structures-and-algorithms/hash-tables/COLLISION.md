🟢 Collision Resolution: Handling Conflicts in Hash Tables
1. What is a Collision?
Definition:
A collision occurs when two or more distinct keys hash to the same index in a hash table.

Example:

text
Hash function: h(key) = key % 5
Table size: 5
Keys: 2, 7, 12

h(2) = 2 % 5 = 2
h(7) = 7 % 5 = 2   ← COLLISION!
h(12) = 12 % 5 = 2 ← COLLISION!

All three keys hash to index 2
Why Collisions Occur:

Finite table size (pigeonhole principle).

Imperfect hash functions.

Structural patterns in input keys.

Intentional attacks (adversarial inputs).

2. Collision Resolution Categories
A. Open Hashing (Closed Addressing)
Collisions stored outside the main table.

B. Closed Hashing (Open Addressing)
Collisions stored inside the table via probing.

3. Separate Chaining (Open Hashing)
Definition:
Each table slot maintains a linked list (chain) of all entries hashing to that slot.

Structure:

text
buckets[0] → null
buckets[1] → [("apple", 5) → ("banana", 3) → null]
buckets[2] → [("cat", 7) → null]
buckets[3] → null
Algorithm:

text
FUNCTION put(key, value)
    idx = hash(key) % size
    IF buckets[idx] == null THEN
        buckets[idx] = []
    END IF
    
    // Search for key in chain
    FOR pair IN buckets[idx] DO
        IF pair.key == key THEN
            pair.value = value  // Update
            RETURN
        END IF
    END FOR
    
    // Key not found; append
    buckets[idx].append((key, value))
    count += 1
    
    IF count / size > loadFactorThreshold THEN
        resize()
    END IF
END FUNCTION

FUNCTION get(key)
    idx = hash(key) % size
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
    idx = hash(key) % size
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
Complexity:

Average: O(1 + α) where α = n/m (load factor).

Worst-case: O(n) if all hash to same bucket.

With good hash function and α ≤ 0.75: O(1) average.

Pros:

Simple implementation.

Graceful degradation (chain length grows, but O(1 + α) maintained).

Easy deletion (just remove from list).

Load factor > 1 possible (multiple entries per bucket).

Cons:

Extra memory for pointers/links.

Poor cache locality (chains scattered in memory).

External memory allocation overhead.

Used in:

Java HashMap.

Python dict (older implementations).

Most textbook implementations.

4. Open Addressing (Closed Hashing)
Definition:
All entries stored in table itself. On collision, probe for next available slot.

General Probing:

text
Sequence: h₁(key), h₁(key) + d₁, h₁(key) + 2d₂, h₁(key) + 3d₃, ...

Where dᵢ depends on probing method.
a. Linear Probing
Probing Sequence:
h(key), h(key)+1, h(key)+2, ..., (mod m)

Algorithm:

text
FUNCTION put(key, value)
    idx = hash(key) % size
    i = 0
    
    WHILE i < size DO
        probeIdx = (idx + i) % size
        
        IF buckets[probeIdx] == null OR buckets[probeIdx] == TOMBSTONE THEN
            buckets[probeIdx] = (key, value)
            count += 1
            RETURN
        ELSE IF buckets[probeIdx].key == key THEN
            buckets[probeIdx].value = value  // Update
            RETURN
        END IF
        
        i += 1
    END WHILE
    
    ERROR "Hash table full"
END FUNCTION

FUNCTION get(key)
    idx = hash(key) % size
    i = 0
    
    WHILE i < size DO
        probeIdx = (idx + i) % size
        
        IF buckets[probeIdx] == null THEN
            RETURN null
        ELSE IF buckets[probeIdx] != TOMBSTONE AND buckets[probeIdx].key == key THEN
            RETURN buckets[probeIdx].value
        END IF
        
        i += 1
    END WHILE
    
    RETURN null
END FUNCTION

FUNCTION remove(key)
    idx = hash(key) % size
    i = 0
    
    WHILE i < size DO
        probeIdx = (idx + i) % size
        
        IF buckets[probeIdx] == null THEN
            RETURN
        ELSE IF buckets[probeIdx] != TOMBSTONE AND buckets[probeIdx].key == key THEN
            buckets[probeIdx] = TOMBSTONE  // Mark deleted
            count -= 1
            RETURN
        END IF
        
        i += 1
    END WHILE
END FUNCTION
Example:

text
Insert: 2, 7, 12 (hash % 5)
h(2) = 2 → buckets[2] = 2
h(7) = 2 → occupied, try 3 → buckets[3] = 7
h(12) = 2 → occupied, try 3 → occupied, try 4 → buckets[4] = 12

Result: [_, _, 2, 7, 12]
Complexity:

Average: O(1/(1-α)) with good hash.

Worst-case: O(n).

Pros:

Excellent cache locality (contiguous entries).

No pointer overhead; compact.

Slightly faster on hit (one memory access).

Cons:

Primary clustering: Collisions cause more collisions nearby.

Chain length grows; access time degrades.

Load factor must stay < 0.5 for good performance.

Deletion complex (requires tombstones).

Clustering Example:

text
Many collisions at index 2 create chain:
[_, _, occupied, occupied, occupied, ...]
New insertions hash to 2 → also fall into cluster
b. Quadratic Probing
Probing Sequence:
h(key), h(key)+1², h(key)+4², h(key)+9², ... (mod m)

Or: h(key) + i² for i = 0, 1, 2, ...

Algorithm:

text
FUNCTION put(key, value)
    idx = hash(key) % size
    i = 0
    
    WHILE i < size DO
        probeIdx = (idx + i*i) % size
        
        IF buckets[probeIdx] == null OR buckets[probeIdx] == TOMBSTONE THEN
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
Example:

text
Insert 12 (h=2)
Probe: 2, 2+1=3, 2+4=6, 2+9=11, ...
Complexity:

Average: O(1/(1-α)).

Better than linear probing in practice.

Pros:

Reduces primary clustering (quadratic jumps).

Better distribution than linear.

Cons:

Secondary clustering: Different keys with same hash follow same probe path.

Requires size = prime to guarantee full coverage.

Slightly worse cache than linear (jumps).

c. Double Hashing
Probing Sequence:
h₁(key), h₁(key) + d·h₂(key), h₁(key) + 2d·h₂(key), ... (mod m)

Where d = 1, 2, 3, ... and h₂(key) = 1 + (key mod (m-1))

Algorithm:

text
FUNCTION hash2(key)
    RETURN 1 + (key % (size - 1))
END FUNCTION

FUNCTION put(key, value)
    idx = hash(key) % size
    step = hash2(key)
    i = 0
    
    WHILE i < size DO
        probeIdx = (idx + i * step) % size
        
        IF buckets[probeIdx] == null OR buckets[probeIdx] == TOMBSTONE THEN
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
Complexity:

Average: O(1/(1-α)).

Best average performance of open addressing methods.

Pros:

Excellent distribution; avoids both primary and secondary clustering.

Good performance across load factors.

Most uniform probing.

Cons:

Requires two hash functions; slightly more complex.

Worse cache locality (random jumps).

Computation overhead (two hashes).

5. Advanced Collision Resolution Techniques
a. Cuckoo Hashing
Definition:
Multiple hash functions. On collision, displace existing entry; recurse.

Algorithm:

text
FUNCTION put(key, value)
    FOR iteration = 1 TO MAX_ITERATIONS DO
        idx1 = hash1(key) % size
        
        IF buckets[idx1] == null THEN
            buckets[idx1] = (key, value)
            RETURN
        END IF
        
        // Displace existing entry
        existingEntry = buckets[idx1]
        buckets[idx1] = (key, value)
        key = existingEntry.key
        
        // Try second hash
        idx2 = hash2(key) % size
        IF buckets[idx2] == null THEN
            buckets[idx2] = (key, existingEntry.value)
            RETURN
        END IF
        
        // Displace again; continue with key from idx2
        temp = buckets[idx2]
        buckets[idx2] = (key, existingEntry.value)
        key = temp.key
    END FOR
    
    // Too many iterations; rehash entire table
    rehash()
    put(key, value)
END FUNCTION

FUNCTION get(key)
    idx1 = hash1(key) % size
    IF buckets[idx1].key == key THEN
        RETURN buckets[idx1].value
    END IF
    
    idx2 = hash2(key) % size
    IF buckets[idx2].key == key THEN
        RETURN buckets[idx2].value
    END IF
    
    RETURN null
END FUNCTION
Complexity:

Search: O(1) worst-case (at most 2 locations).

Insert: O(1) amortized; O(n) on rehash.

Pros:

O(1) worst-case search (no clustering).

High space utilization (load factor up to 0.5+).

Cons:

Complex implementation.

Insertion worst-case O(n) (rehashing).

Rare in practice; primarily theoretical.

b. Robin Hood Hashing
Definition:
Minimize variance in collision chain lengths by "stealing" from richer chains.

Principle:

text
Track probe distance (how far from home position)
On collision:
  - If new entry has longer distance than existing
  - Swap; displaced entry continues
  - Reduces worst-case chain length
Benefit:

More uniform access time.

Better worst-case than standard linear probing.

c. Hopscotch Hashing
Definition:
Limit search radius to neighborhood around home position.

Algorithm:

text
Desired position: h(key)
Search within k positions of home
If not found, shift entries to maintain invariant
Benefit:

Excellent cache locality.

Bounded neighborhood search.

6. Comparison of Strategies
Strategy	Avg Search	Worst Search	Avg Insert	Space	Cache	Deletion
Chaining	O(1+α)	O(n)	O(1+α)	Higher	Poor	Easy
Linear Probing	O(1/(1-α))	O(n)	O(1/(1-α))	Low	Excellent	Tombstone
Quadratic Probing	O(1/(1-α))	O(n)	O(1/(1-α))	Low	Good	Tombstone
Double Hashing	O(1/(1-α))	O(n)	O(1/(1-α))	Low	Fair	Tombstone
Cuckoo	O(1) worst	O(1)	O(1) amortized	Low	Good	Easy
7. Load Factor Impact
Load Factor (α) = n / m (n = entries, m = table size)

text
Separate Chaining:
  α = 0.5: avg chain ≈ 0.5; good
  α = 1.0: avg chain ≈ 1.0; acceptable
  α = 2.0: avg chain ≈ 2.0; degraded

Open Addressing (Linear Probing):
  α = 0.5: avg probe ≈ 1.5; good
  α = 0.75: avg probe ≈ 3; acceptable
  α = 0.9: avg probe ≈ 10; poor
  α → 1.0: avg probe → ∞

Critical: Keep α ≤ 0.75 for open addressing.
8. When Collisions Occur
Expected:

text
Birthday Paradox: With n elements and m slots,
probability of collision ≈ 1 - e^(-n²/2m)

For m = 10K, n = 100:
  P(collision) ≈ 0.5%

For m = 10K, n = 1000:
  P(collision) ≈ 50%
Adversarial:

text
Attacker crafts keys to hash same; all to index 0
Result: All operations O(n)
Solution: Randomized hash function (universal hashing)
9. Best Practices
Choosing Collision Resolution:
Scenario	Best Choice
General purpose	Separate chaining
Performance-critical	Double hashing
Memory-constrained	Linear probing (low load)
Need O(1) worst-case	Cuckoo hashing
Simple implementation	Linear probing
Frequent deletion	Separate chaining
Load Factor Management:
text
Separate Chaining:
  - Keep α ≤ 0.75
  - Resize when α > 0.75
  - newSize = oldSize * 2

Open Addressing:
  - Keep α ≤ 0.5
  - Resize when α > 0.5
  - newSize = nextPrime(oldSize * 2)
Hash Function:
Distribute uniformly.

Avalanche property (small input change → large hash change).

Fast computation.

Avoid modulo prime adjacent to power of 2.

10. Real-World Patterns
Memcached (Facebook):

Separate chaining with consistent hashing.

Distributed across cluster nodes.

Java HashMap:

Separate chaining (default).

Resizes when α > 0.75.

Uses object.hashCode() for all objects.

C++ unordered_map:

Typically open addressing (linear or quadratic).

Implementation-dependent; usually good defaults.

Python dict:

Open addressing (CPython 3.6+).

Compact representation; highly optimized.

11. Interview Patterns
Explain collision: When two keys hash to same index.

Compare chaining vs open addressing: Trade-offs.

Probe sequences: Linear, quadratic, double hashing differences.

Load factor: Why resize; when to resize.

Clustering: Primary vs secondary; causes and solutions.

Deletion: Why tombstones in open addressing.

12. Summary
Collision is inevitable in finite hash tables. Effective resolution maintains O(1) average operations. Separate chaining simple and practical; open addressing cache-friendly. Choose based on workload (frequent deletion favors chaining; performance favors open addressing with low load factor).

Master collision resolution to design hash tables that stay performant under any load—from casual use to adversarial attacks!