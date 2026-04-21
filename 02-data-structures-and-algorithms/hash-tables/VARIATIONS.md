🟢 Hash Table Variations: Types, Structures & Real-World Applications
1. Collision Resolution Strategies
a. Separate Chaining (Closed Addressing)
Definition:
Each bucket stores a linked list (or vector) of entries. Multiple entries with same hash coexist in same bucket.

Structure:

text
buckets[0] → [("a", 1) → ("x", 10) → null]
buckets[1] → null
buckets[2] → [("b", 2) → null]
buckets[3] → [("c", 3) → ("d", 4) → null]
Pros:

Simple implementation.

Performance degrades gracefully with high load factor.

Easy deletion (just remove from chain).

Hash function independence (good hash not critical).

Cons:

Extra memory for pointers/links.

Poor cache locality (chains scattered in memory).

External memory allocation.

Complexity:

Average: O(1 + α) where α = load factor (n/m).

Worst-case: O(n) if all hash to same bucket.

Used in:

Java HashMap, Python dict (before optimization).

Most educational implementations.

b. Open Addressing (Closed Hashing)
Definition:
All entries stored in array itself. On collision, find alternate empty slot via probing sequence.

Probing Strategies:

1. Linear Probing

text
Probe sequence: h(key), h(key)+1, h(key)+2, ...

Insert "apple" (hash 4 → slot 4 occupied):
  Try 4 (occupied)
  Try 5 (empty) → insert

Get "apple":
  Try 4 (not apple) → try 5 (found!)
Pros: Simple, excellent cache locality.

Cons: Primary clustering (collisions cause more collisions).

Complexity: O(1 + α) average, but with high constants.

2. Quadratic Probing

text
Probe sequence: h(key), h(key)+1², h(key)+4², h(key)+9², ...

Reduces clustering vs linear probing.
Requires table size = prime to guarantee coverage.
Pros: Reduces clustering vs linear probing.

Cons: Secondary clustering (different keys same probe path).

Complexity: O(1 + α) average.

3. Double Hashing

text
Probe sequence: h1(key), h1(key) + d*h2(key), h1(key) + 2d*h2(key), ...

h2(key) = 1 + (key mod (m-1))  // Ensures coprimality

Best distribution among open addressing schemes.
Pros: Best distribution; avoids both primary and secondary clustering.

Cons: Slightly more complex; need two hash functions.

Complexity: O(1 + α) average.

Open Addressing Pros/Cons (overall):

Pros: Better cache locality, no pointer overhead, compact.

Cons: Difficult deletion (requires tombstones), poor worst-case.

Used in:

C++ unordered_map (typically).

System-level hashtables.

Performance-critical applications.

c. Cuckoo Hashing
Definition:
Multiple hash functions. If collision, displace existing entry to another location.

Algorithm:

text
INSERT key:
  IF h1(key) empty:
    INSERT at h1(key)
  ELSE IF h2(key) empty:
    INSERT at h2(key)
  ELSE:
    Displace item at h1(key); recurse with displaced item
    (with limit to prevent infinite loop)
Properties:

Worst-case O(1) lookup: No clustering; entries spread evenly.

Amortized O(1) insertion: Expected case; worst-case rehashing.

Memory: Higher load factor (up to 0.5+ vs 0.75 for chaining).

Pros:

O(1) worst-case lookup (no clustering).

High space efficiency.

Cons:

Complex implementation.

Insertion worst-case O(n) (rehashing needed).

Rare in practice.

Used in:

Theoretical computer science.

Specialized systems requiring O(1) worst-case lookup.

d. Robin Hood Hashing
Definition:
Minimize variance in collision chain lengths by "stealing" from richer chains.

Algorithm:

text
On collision: Compare probe distances
  IF new entry has farther distance than existing:
    Swap; continue with displaced entry
  ELSE:
    Probe further
Benefit:

Reduces worst-case clustering.

More uniform access time.

Used in:

Optimized implementations (some C++ libraries).

When predictable latency critical.

e. Hopscotch Hashing
Definition:
Keep entries near their home position; limit search radius.

Algorithm:

text
INSERT:
  - Desired position: h(key)
  - Search within k positions of home
  - If not found, "hop" closer from neighbors
  - Shift entries to maintain invariant
Benefit:

Excellent cache locality.

Search bounded within small neighborhood.

Used in:

GPU hashtables.

Cache-optimized systems.

2. Hash Function Variations
a. Simple Hash Functions
Modulo:

text
h(key) = key mod m
Problem: Poor distribution if key's LSBs correlated.
Multiplicative:

text
h(key) = ⌊m * (k*A mod 1)⌋ where A = (√5 - 1)/2 ≈ 0.618
Better distribution; less clustering on structured keys.
Folding:

text
Divide key into parts; XOR or sum parts.
Used for composite keys.
b. String Hash Functions
Polynomial Rolling Hash:

text
h(s) = (s[0]*p^(n-1) + s[1]*p^(n-2) + ... + s[n-1]) mod m
p = prime (typically 31 or 37)

Advantage: Fast; good distribution.
Disadvantage: Possible hash collisions.
DJB2:

text
h(s) = 5381
for each char c in s:
  h = ((h << 5) + h) + c
return |h| mod m
Cryptographic (SHA-256, MD5):

text
h = SHA256(key)
Advantage: Excellent distribution; collision-resistant.
Disadvantage: Slow; overkill for hashtables.
c. Universal Hashing
Definition:
Randomized hash function from universal family; worst-case O(log n) with high probability.

Property:

text
For any two distinct keys k1, k2:
Pr[h(k1) = h(k2)] ≤ 1/m
Used in:

Theoretical analysis.

Load-balanced systems.

3. Specialized Hash Table Structures
a. Hash Set (Unordered Set)
Definition: Hash table storing only keys (no values).

Operations: Insert, remove, contains.

Used for: Deduplication, membership testing, fast lookup.

Examples: Java HashSet, Python set, C++ unordered_set.

b. Hash Map (Dictionary)
Definition: Hash table storing key-value pairs.

Operations: Insert, remove, lookup value by key.

Examples: Java HashMap, Python dict, C++ unordered_map.

c. Multi-Map
Definition: Hash table allowing multiple values per key.

Structure: Key → list of values.

Used for: Grouping related items (e.g., URLs by domain).

d. Bidirectional Hash Table (BiMap)
Definition: Maintains both key→value and value→key mappings.

Operations: Insert, lookup by key or value.

Used for: Language translation (word↔translation), ID mapping.

e. LRU Cache
Structure: Hash table + doubly-linked list.

Operations: O(1) get, put, eviction.

Used for: Browser caching, database buffers.

f. Bloom Filter
Definition: Probabilistic set; space-efficient membership testing.

False positives: Possible; false negatives: impossible.

Space: O(m) bits for n elements (m << n*log n).

Used for: Spell checking, URL deduplication, cache filtering.

g. Count-Min Sketch
Definition: Approximate frequency counting with small memory.

Operations: Insert, query frequency (approximate).

Space: O(ε⁻¹ log n) for ε-approximate counts.

Used for: Streaming analytics, top-K queries.

h. Consistent Hashing
Definition: Hash function that minimizes remapping when nodes added/removed.

Structure: Virtual ring; nodes and keys both hashed to ring.

On node add/remove: Only O(k/n) keys affected (k=total, n=nodes).

Used for: Distributed caches (Memcached, Redis), load balancing, DHTs.

i. Linear Hashing
Definition: Dynamic growing hashtable; grows one bucket at a time.

Advantage: No full rehash; incremental growth.

Used for: Databases, file systems.

j. Extendible Hashing
Definition: Directory-based; directory doubles when overflow.

Advantage: Handles arbitrary number of insertions; quick access.

Used for: Database B-trees, file indexing.

4. Language Implementations
Python
python
# dict (compact hash table, CPython 3.6+)
d = {}
d['key'] = 'value'
value = d['key']
if 'key' in d:
    ...

# Implementation: Open addressing with linear probing
# Features: Ordered (insertion order preserved)
Java
java
// HashMap (separate chaining)
HashMap<String, Integer> map = new HashMap<>();
map.put("key", 123);
int value = map.getOrDefault("key", -1);

// Hashtable (legacy; synchronized)
Hashtable<String, Integer> table = new Hashtable<>();

// Properties: Load factor = 0.75; initial capacity = 16
C++
cpp
// unordered_map (typically open addressing)
#include <unordered_map>
std::unordered_map<std::string, int> map;
map["key"] = 123;
auto it = map.find("key");

// unordered_set (keys only)
std::unordered_set<int> set;
set.insert(42);
C#
csharp
// Dictionary (separate chaining)
Dictionary<string, int> dict = new Dictionary<string, int>();
dict["key"] = 123;
dict.TryGetValue("key", out int value);

// HashSet (keys only)
HashSet<int> set = new HashSet<int>();
set.Add(42);
Go
go
// map (built-in; open addressing)
m := make(map[string]int)
m["key"] = 123
value := m["key"]
if val, ok := m["key"]; ok {
    ...
}
5. Choosing the Right Hash Table Variation
Use Case	Variant	Why
General purpose	Separate chaining	Simple, standard, good performance
Performance-critical	Open addressing	Better cache locality
High load factor	Cuckoo hashing	O(1) worst-case lookup
Memory constrained	Bloom filter	O(m) bits vs O(n) space
Distributed systems	Consistent hashing	Minimize remapping on scale
LRU caching	Hash + doubly-linked list	O(1) all ops
Approximate counting	Count-Min sketch	Sublinear space
Ordered iteration	Tree-based (BST)	O(log n) ops + sorted
6. Performance Comparison
Operation	Chaining	Linear Probing	Quadratic	Double Hash	Cuckoo
Insert avg	O(1+α)	O(1+α)	O(1+α)	O(1+α)	O(1) amortized
Insert worst	O(n)	O(n)	O(n)	O(n)	O(n)
Search avg	O(1+α)	O(1+α)	O(1+α)	O(1+α)	O(1) worst
Search worst	O(n)	O(n)	O(n)	O(n)	O(1)
Delete avg	O(1+α)	O(1+α) tombstone	O(1+α) tombstone	O(1+α) tombstone	O(1)
Cache friendly	Poor	Excellent	Good	Good	Excellent
Max load factor	0.75+	0.5	0.5	0.5	0.5
7. Advanced Topics
Linear Hashing with Progressive Overflow
Grows incrementally without full rehash.

Directory-based; handles dynamic resizing.

Extendible Hashing
Directory of buckets; directory grows when overflow.

Used in B-trees and file systems.

Dynamic Perfect Hashing
Maintains O(1) worst-case lookup dynamically.

Complex implementation; theoretical interest.

Distributed Hash Tables (DHTs)
Chord, Kademlia, Pastry algorithms.

Used in peer-to-peer systems.

8. Real-World Variant Selection
Browser Cache:

LRU + Hash: O(1) lookup and eviction.

Memcached:

Separate chaining + Consistent hashing.

Distributed; minimizes remapping on scale.

Database Index:

Hash index for exact match; B-tree for range.

Hybrid: hash for primary key, tree for secondary.

Compiler Symbol Table:

Separate chaining; simple, fast lookup.

Rate Limiting:

Hash table: user_id → token count.

Expires old entries via TTL.

9. Interview Variant Patterns
Separate chaining basics: Implement put/get/remove.

Collision resolution: Compare linear vs double hashing.

Load factor: When to resize; cost amortization.

LRU cache: Hash + doubly-linked list.

Bloom filter: Space efficiency, false positive tuning.

Consistent hashing: Distributed systems, remapping minimization.

10. Best Practices
Default: Use language's built-in (HashMap, dict, unordered_map).

Custom: Only if special requirements (e.g., lock-free, persistent).

Collision resolution: Separate chaining for simplicity; open addressing for cache.

Hash function: Use language's default unless domain-specific knowledge.

Load factor: Keep ≤ 0.75 for good performance.

Testing: Test with high collision scenarios, empty, single element.

Master hash table variations to choose the perfect structure for any scenario—from standard dictionaries to specialized caches to distributed systems at scale!

