🟢 Hash Tables: Comprehensive Mastery from Foundations to Advanced Systems
1. What is a Hash Table?
A hash table is a data structure that implements an associative array—a structure mapping keys to values using a hash function to compute indices into an array of buckets or slots.

Key Characteristics:

Hash Function: Converts key to array index.

Collision Resolution: Handles multiple keys mapping to same index.

Dynamic Resizing: Grows/shrinks as elements added/removed.

Average O(1) Operations: Insert, search, delete in constant time on average.

2. Core Components
a. Hash Function
Maps key (any type) to index (0 to m-1) where m = table size.

Properties:

Deterministic: Same key always produces same hash.

Uniform distribution: Hash values spread evenly across range.

Fast computation: O(1) time.

Examples:

String: sum of ASCII values, polynomial rolling hash, cryptographic (SHA).

Integer: modulo m, multiplicative, folding.

b. Collision Resolution
Separate Chaining: Each slot stores linked list of entries hashing to it.

Open Addressing: Find alternate slot if collision occurs.

Linear probing, quadratic probing, double hashing.

Cuckoo Hashing: Multiple hash functions; displace existing entry if needed.

c. Load Factor
α
=
n
/
m
α=n/m (n = entries, m = table size).

Determines when to resize.

Higher 
α
α = more collisions; lower = more space wasted.

Typical range: 0.5 to 0.75.

d. Resize (Rehashing)
When load factor exceeds threshold, allocate larger table.

Re-insert all entries with new hash values.

Time: O(n), amortized O(1) per insertion.

3. Collision Resolution Strategies
a. Separate Chaining
text
Hash table with m=5 slots:

  Index 0: [null]
  Index 1: [("apple", 5) → ("banana", 3)]
  Index 2: [("cat", 7)]
  Index 3: [null]
  Index 4: [("dog", 2) → ("elephant", 4)]
Pros: Simple, performance degrades gracefully, easy deletion.

Cons: Extra memory for pointers.

Used in: Most production systems (Java HashMap, Python dict internally for large loads).

b. Open Addressing – Linear Probing
text
On collision, try next slot: h(key), h(key)+1, h(key)+2, ...

Insert "apple" (hash 1), "banana" (hash 1):
  [_, "apple", "banana", _, _]
Pros: Better cache locality, no extra pointers.

Cons: Clustering (collisions cause more collisions), deletion complex.

c. Open Addressing – Quadratic Probing
text
On collision, try: h(key), h(key)+1², h(key)+4², h(key)+9², ...
Reduces clustering vs linear probing.
d. Double Hashing
text
Use two hash functions: h1(key), h1(key) + d*h2(key), h1(key) + 2d*h2(key), ...
Better distribution; avoids clustering.
e. Cuckoo Hashing
text
Multiple hash functions; displace existing entry if needed.
Guarantees O(1) worst-case lookup (after insertion).
Complex implementation; rare in practice.
4. Hash Function Design
a. Good Hash Function Properties
Deterministic: f(x) always same for given x.

Uniform: Minimizes collisions; even distribution.

Fast: O(1) computation.

Avalanche: Small input change → large output change.

b. Bad Hash Functions
Modulo prime close to power of 2 (bias).

Simple sum of digits (many collisions for similar keys).

Non-uniform (clusters in certain ranges).

c. Hash Functions by Type
Integers: Modulo, multiplicative, folding.

Strings: Polynomial rolling hash, DJB2, FNV-1a.

Objects: Combine field hashes (Java hashCode()).

Cryptographic: SHA-256, MD5 (for security; slower).

5. Load Factor and Resizing
a. Why Resizing?
As 
α
α increases, collision rate grows.

α
α > 0.75: Expected chain length ~0.75; degraded performance.

Solution: Double table size when 
α
α > threshold.

b. Resizing Strategy
text
IF loadFactor > threshold THEN
    newSize = oldSize * 2
    newTable = allocateNewTable(newSize)
    FOR each entry IN oldTable DO
        newTable[hash(key) % newSize] = entry
    END FOR
    table = newTable
END IF
c. Cost Analysis
Resize cost: O(n).

Amortized cost per insertion: O(1) (expensive resizes amortized over many insertions).

6. Complexity Analysis
Operation	Average	Worst Case
Search	O(1)	O(n)
Insert	O(1)	O(n)
Delete	O(1)	O(n)
Space	O(n)	O(n)
Worst case (all collisions) rare in practice with good hash function.

7. Applications and Use Cases
a. Caching
Web server caches responses by URL (key).

CPU caches via memory addresses.

b. Symbol Tables (Compilers)
Variable names → types, scopes.

Function names → addresses.

c. Duplicate Detection
Check if element seen before: O(1).

d. Frequency Counting
Count word frequencies in text.

e. Database Indexing
Primary index: key → record address.

f. Sets (Membership Testing)
Java HashSet, Python set.

O(1) average membership check.

g. Associative Arrays / Dictionaries
Python dict, Java HashMap, C++ unordered_map.

h. Password Storage (with cryptographic hash)
Hash(password) stored, not plaintext.

8. Advanced Variations
a. Perfect Hash Table
Hash function guarantees no collisions (offline).

Requires knowing all keys upfront.

Used in: Compiler keyword tables, switch statement optimization.

b. Bloom Filter
Probabilistic data structure; space-efficient set membership.

False positives possible; no false negatives.

Used in: Spell checkers, URL duplicate detection, caches.

c. Count-Min Sketch
Approximate frequency counting with small memory.

Used in: Streaming data, top-K queries.

d. Consistent Hashing
Minimizes remapping when table resizes.

Used in: Distributed caches, load balancing, DHTs.

e. LRU Cache with Hash Table + Doubly-Linked List
Hash table for O(1) lookup.

Doubly-linked list for O(1) eviction/reordering.

Used in: Browser caches, CPU caches, database buffers.

f. Linear Probing with Tombstones
Mark deleted entries; skip during search.

Avoids expensive rehashing on delete.

9. FAANG and Top Company Patterns
Common Interview Questions:

Two Sum: Hash table for complement lookup; O(n) time.

Anagrams: Hash by sorted characters; group anagrams.

LRU Cache: Hash + doubly-linked list; O(1) all operations.

Design HashMap: Implement from scratch (separate chaining).

Happy Number: Hash set for cycle detection.

Company-Specific:

Google: Hash tables fundamentals, collision resolution, bloom filters.

Amazon: LRU cache design, consistent hashing.

Facebook: Hash-based duplicate detection, frequency counting.

Microsoft: Hash implementation, collision resolution strategies.

Apple: Hash function design, memory optimization.

10. Common Pitfalls
Poor hash function: Clustered collisions → degraded performance.

Not resizing: Load factor grows unbounded; O(n) operations.

Mutable keys: Changing key after insert breaks table.

Not handling collisions: Overwrites entries.

Infinite loops: Bad collision resolution (e.g., quadratic probing with bad parameters).

11. Best Practices
Use language's built-in hash tables (HashMap, dict).

Immutable keys (strings, integers) safer than mutable objects.

Prime table sizes reduce clustering (some implementations).

Load factor 0.5-0.75 balances speed vs space.

Cryptographic hashing for sensitive data (passwords).

12. Language Implementations
Python: dict (open addressing; CPython 3.6+).

Java: HashMap (separate chaining; default 75% load factor).

C++: unordered_map (implementation-dependent; usually chaining).

C#: Dictionary<K, V> (chaining; resizes at 75%).

JavaScript: Map, Object (engine-optimized).

13. Advanced Topics
Universal Hashing: Randomized hash function; worst-case O(log n).

Robin Hood Hashing: Minimize variance in collision chain lengths.

Hopscotch Hashing: Neighbor-based hashing; good cache locality.

Distributed Hash Tables (DHT): Chord, Kademlia (P2P systems).

Cryptographic Hash Functions: SHA-256, BLAKE3 (security-focused).

14. Summary and Learning Outcomes
Understand hash function design; collision resolution trade-offs.

Implement hash table from scratch (separate chaining).

Analyze complexity; understand when O(1) average becomes O(n) worst-case.

Apply to solve: two-sum, anagrams, LRU cache, frequency counting.

Recognize real-world use: caching, symbol tables, databases.

Avoid common pitfalls: poor hash functions, unbounded load factors, mutable keys.

Hash tables are the invisible foundation of modern computing—powering caches, databases, compilers, and real-time systems. Master them to build blazing-fast systems and excel in interviews!