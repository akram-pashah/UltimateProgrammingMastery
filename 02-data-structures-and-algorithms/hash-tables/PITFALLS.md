🟢 Hash Table Pitfalls: Common Bugs, Edge Cases & Defensive Coding
1. Poor Hash Function Design
Description:
Hash function distributes keys unevenly; clusters data into few buckets instead of spreading across table.

Impact:
High collision rate; O(n) operations instead of O(1); performance degrades.

Prevention:
Use language's built-in hash (Python dict, Java HashMap); avoid modulo on small primes; test distribution on real data.

text
// Bug: Simple modulo with structured keys
FUNCTION badHash(key)
    RETURN key % 16  // Poor for sequential keys: 0, 16, 32... hash to 0

// Fixed: Better distribution
FUNCTION goodHash(key)
    hash = key * 2654435761  // Multiplicative constant
    RETURN hash % 16
END FUNCTION
2. Ignoring Load Factor
Description:
Not resizing table as it fills; load factor grows unbounded.

Impact:
Collision chains grow long; O(1+α) degrades to O(n) as α increases.

Prevention:
Monitor load factor; resize when α > 0.75 (chaining) or α > 0.5 (open addressing).

text
// Bug: Never resize
FUNCTION put(key, value)
    bucket[hash(key) % size] = (key, value)
    // loadFactor increases; performance degrades

// Fixed: Resize when needed
FUNCTION put(key, value)
    bucket[hash(key) % size] = (key, value)
    count += 1
    IF count / size > 0.75 THEN
        resize(size * 2)
    END IF
END FUNCTION
3. Using Mutable Keys
Description:
Key changes after insertion; hash value changes; lookup fails.

Impact:
Entry becomes unreachable; lost data; silent data corruption.

Prevention:
Use immutable keys (strings, integers, tuples); never modify key after insert.

text
// Bug: Mutable key (in Python)
myList = [1, 2, 3]
table[myList] = "value"  // TypeError: lists unhashable
myList.append(4)  // If it were allowed, lookup would fail

// Fixed: Immutable key
myTuple = (1, 2, 3)
table[myTuple] = "value"  // Works; hashable
4. Not Handling Null/None Keys
Description:
Attempting to use null as key in language that doesn't support it.

Impact:
Runtime error, exception, crash.

Prevention:
Check for null before insert; use language-appropriate null handling.

text
// Bug: Assume null allowed
FUNCTION put(key, value)
    hash = hashFunction(key)  // May crash if key == null

// Fixed: Check null
FUNCTION put(key, value)
    IF key == null THEN
        ERROR "Null keys not supported"
    END IF
    hash = hashFunction(key)
END FUNCTION
5. Trying to Hash Unhashable Objects
Description:
Using mutable or complex objects as keys (lists, sets, custom objects without proper hashCode).

Impact:
TypeError (Python), incorrect behavior (JavaScript), unexpected results.

Prevention:
Use hashable types; implement hashCode() and equals() for custom objects (Java).

text
// Bug (Python): Unhashable mutable
myDict = {}
myList = [1, 2, 3]
myDict[myList] = "value"  // TypeError: unhashable type

// Fixed: Use tuple instead
myDict[(1, 2, 3)] = "value"  // Works
6. Ineffective Collision Resolution
Description:
Collision strategy implemented incorrectly; wrong data structures or logic.

Impact:
Data loss, infinite loops, incorrect lookups.

Prevention:
Test collision handling; verify probe sequences; handle edge cases (tombstones, wrapping).

text
// Bug: Linear probing without wrapping
FUNCTION get(key)
    idx = hash(key) % size
    WHILE bucket[idx] != null DO
        IF bucket[idx].key == key THEN
            RETURN bucket[idx].value
        END IF
        idx += 1  // May go out of bounds!

// Fixed: Use modulo to wrap
        idx = (idx + 1) % size
END FUNCTION
7. Forgetting Tombstones in Open Addressing
Description:
Deleting entry in open addressing; leaves hole; subsequent probes fail.

Impact:
Lookup misses; entries unreachable; deletion breaks table.

Prevention:
Mark deleted as TOMBSTONE; skip during search; clean up periodically.

text
// Bug: Delete without marking
FUNCTION remove(key)
    idx = hash(key) % size
    IF bucket[idx].key == key THEN
        bucket[idx] = null  // Hole!

// Fixed: Mark tombstone
        bucket[idx] = TOMBSTONE
    END IF
END FUNCTION
8. Off-by-One Errors in Resizing
Description:
Wrong new size calculation; resize condition off by one.

Impact:
Resize triggers too early/late; performance degradation; memory waste.

Prevention:
Double size (power of 2); use clear threshold; test boundary conditions.

text
// Bug: Wrong threshold
IF count >= size THEN  // Too late; already full

// Fixed: Clear threshold
IF count / size > 0.75 THEN  // Resize before full
END IF
9. Not Handling Hash Collisions on Initial Insert
Description:
First insertion ignores that slot may be occupied (if not checking).

Impact:
Data overwritten; lost entries.

Prevention:
Always check for existing key before overwriting; search chain/probe sequence.

text
// Bug: Overwrite without checking
FUNCTION put(key, value)
    idx = hash(key) % size
    bucket[idx] = (key, value)  // May overwrite!

// Fixed: Check for existing key
    FOR pair IN bucket[idx] DO
        IF pair.key == key THEN
            pair.value = value
            RETURN
        END IF
    END FOR
    bucket[idx].append((key, value))
END FUNCTION
10. Memory Allocation Failures
Description:
Not handling allocation failure during resize; memory exhausted.

Impact:
Program crash; out of memory exception.

Prevention:
Try-catch around resize; graceful degradation or error handling.

text
// Bug: No error handling
FUNCTION resize(newSize)
    newBucket = ALLOCATE(newSize)  // May fail

// Fixed: Handle failure
    IF newBucket == null THEN
        ERROR "Resize failed; out of memory"
    END IF
END FUNCTION
11. Cache Misses from Poor Locality
Description:
Linked list chains scattered in memory; poor cache performance.

Impact:
Slow lookup despite low collision rate; L1/L2 cache misses.

Prevention:
Use contiguous array-backed chains; or open addressing for better cache.

text
// Poor cache locality (separate chaining with linked list)
bucket[i] → linked_list (scattered memory)

// Better locality (vector-backed chains)
bucket[i] → vector (contiguous; better cache)
12. Integer Overflow in Hash Computation
Description:
Hash computation overflows; wraps around; different keys same hash.

Impact:
Unexpected collisions; data corruption.

Prevention:
Use unsigned integers; handle overflow in language appropriately.

text
// Bug (C): Integer overflow
FUNCTION hash(key)
    hash = 0
    FOR char IN key DO
        hash = hash * 31 + char  // May overflow

// Fixed: Use modulo to prevent overflow
        hash = (hash * 31 + char) % PRIME
    END FOR
    RETURN hash
END FUNCTION
13. Edge Cases: Empty, Single, Two-Element Tables
Description:
Not testing empty table, single entry, or near-capacity table.

Impact:
Index errors, crashes on edge cases.

Prevention:
Always test: empty, one entry, two entries, full table, after resize.

text
// Test cases
put(key1, val1); get(key1) == val1
remove(key1); get(key1) == null
put(key1, val1); put(key1, val2); get(key1) == val2
14. Comparison Operator Issues
Description:
Wrong comparison (== vs equals()); fails to find existing key.

Impact:
Duplicate keys inserted; data loss on update.

Prevention:
Override equals() for custom objects; use consistent comparator.

text
// Bug (Java): Using == for object comparison
IF bucket[idx].key == newKey THEN  // Reference equality, not value

// Fixed: Use equals()
    IF bucket[idx].key.equals(newKey) THEN
    END IF
END FUNCTION
15. Not Handling Worst-Case Adversarial Input
Description:
Attacker sends keys hashing to same bucket; O(1) becomes O(n).

Impact:
Denial of service (DoS); performance attack.

Prevention:
Use randomized hash (universal hashing); cryptographic hash for sensitive data.

text
// Vulnerable to adversarial input
hash = (key % PRIME) % size

// Resistant: Randomized hash
FUNCTION randomizedHash(key)
    RETURN (key * randomValue + randomOffset) % PRIME
END FUNCTION
16. Forgetting to Clear Stale Data on Resize
Description:
Old bucket references persist after resize; memory leak.

Impact:
Memory waste; GC pressure (Java/Python).

Prevention:
Explicitly null old buckets; clear references after transfer.

text
// Bug: Old buckets leak memory
FUNCTION resize(newSize)
    newBuckets = ALLOCATE(newSize)
    FOR entry IN oldBuckets DO
        // Transfer to newBuckets
    END FOR
    buckets = newBuckets
    // oldBuckets not cleared; memory leak

// Fixed: Clear old reference
    oldBuckets = null  // Allow GC
END FUNCTION
17. Modifying Table During Iteration
Description:
Inserting/deleting while iterating; invalidates iterators.

Impact:
Skip elements, revisit elements, crashes.

Prevention:
Collect keys to modify; iterate copy; or use safe iterators.

text
// Bug: Modify during iteration
FOR key IN table.keys() DO
    IF someCondition(key) THEN
        table.remove(key)  // Invalidates iterator

// Fixed: Collect changes, apply after
keysToRemove = []
FOR key IN table.keys() DO
    IF someCondition(key) THEN
        keysToRemove.append(key)
    END IF
END FOR
FOR key IN keysToRemove DO
    table.remove(key)
END FOR
18. Not Testing With Many Collisions
Description:
Test only with good-distribution keys; miss collision handling bugs.

Impact:
Bugs hidden; fail in production with adversarial data.

Prevention:
Test with keys that hash to same bucket; force high collision rate.

text
// Test collision handling
FOR i = 0 TO 1000 DO
    key = i * TABLE_SIZE + 0  // All hash to same bucket
    table.put(key, i)
END FOR
// Verify all 1000 entries retrievable
19. Race Conditions in Concurrent Access
Description:
Multiple threads accessing/modifying without synchronization.

Impact:
Data corruption, lost updates, crashes.

Prevention:
Synchronize access; use concurrent hash table implementations (ConcurrentHashMap).

text
// Bug: Unsafe concurrent access
FUNCTION put(key, value)
    bucket[hash(key)] = (key, value)  // Not atomic

// Fixed: Synchronized (or use ConcurrentHashMap)
SYNCHRONIZED FUNCTION put(key, value)
    bucket[hash(key)] = (key, value)
END FUNCTION
20. Interview-Specific Pitfalls
Forgetting null checks: Always validate input.

Off-by-one indexing: Careful with modulo and wrapping.

Not explaining trade-offs: Discuss chaining vs open addressing.

Assuming perfect hash: Acknowledge collisions are inevitable.

Ignoring load factor: Remember O(1 + α).

21. Best Practices
Use language's built-in hash table (safer, faster).

Test thoroughly with edge cases and collision scenarios.

Monitor load factor; resize proactively.

Use immutable keys; or ensure hashCode/equals consistency.

Profile for cache misses and memory efficiency.

Handle null keys appropriately for your language.

Master these pitfalls to write production-grade hash tables, acing interviews and avoiding subtle bugs that waste weeks of debugging!