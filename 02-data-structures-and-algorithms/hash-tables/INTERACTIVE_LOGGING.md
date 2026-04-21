🟢 Interactive Logging: Hash Tables – Step-by-Step Operation Traces
1. Hash Table with Separate Chaining (Basic Operations)
text
[Initialize Hash Table]
size = 4
buckets = [null, null, null, null]
count = 0
loadFactor = 0.0

[Put "apple", 5]
  hash("apple") = (1+16+16+12+5) % 4 = 50 % 4 = 2
  idx = 2
  buckets[2] = [("apple", 5)]
  count = 1
  loadFactor = 1/4 = 0.25
  
  Buckets state:
    [0] -> null
    [1] -> null
    [2] -> [("apple", 5)]
    [3] -> null

[Put "grape", 7]
  hash("grape") = (7+18+1+16+5) % 4 = 47 % 4 = 3
  idx = 3
  buckets[3] = [("grape", 7)]
  count = 2
  loadFactor = 2/4 = 0.50
  
  Buckets state:
    [0] -> null
    [1] -> null
    [2] -> [("apple", 5)]
    [3] -> [("grape", 7)]

[Put "banana", 3]
  hash("banana") = (2+1+14+1+14+1) % 4 = 33 % 4 = 1
  idx = 1
  buckets[1] = [("banana", 3)]
  count = 3
  loadFactor = 3/4 = 0.75
  
  Buckets state:
    [0] -> null
    [1] -> [("banana", 3)]
    [2] -> [("apple", 5)]
    [3] -> [("grape", 7)]

[Put "apricot", 2]
  hash("apricot") = (1+16+18+9+3+15+20) % 4 = 82 % 4 = 2
  idx = 2
  Collision! Search bucket 2 for "apricot"
  buckets[2] = [("apple", 5), ("apricot", 2)]
  count = 4
  loadFactor = 4/4 = 1.0 > 0.75 → RESIZE
  
  [Resize triggered]
    newSize = 4 * 2 = 8
    oldBuckets = [null, [("banana", 3)], [("apple", 5), ("apricot", 2)], [("grape", 7)]]
    
    Rehash all entries:
      hash("banana") % 8 = 33 % 8 = 1
      hash("apple") % 8 = 50 % 8 = 2
      hash("apricot") % 8 = 82 % 8 = 2
      hash("grape") % 8 = 47 % 8 = 7
    
    newBuckets:
      [0] -> null
      [1] -> [("banana", 3)]
      [2] -> [("apple", 5), ("apricot", 2)]
      [3] -> null
      [4] -> null
      [5] -> null
      [6] -> null
      [7] -> [("grape", 7)]
  
  loadFactor = 4/8 = 0.50

[Get "apple"]
  hash("apple") % 8 = 50 % 8 = 2
  idx = 2
  Search bucket[2]: [("apple", 5), ("apricot", 2)]
  Found! Return 5

[Get "unknown"]
  hash("unknown") % 8 = ??? % 8 = ?
  idx = ?
  Search bucket[idx]: not found
  Return null

[Remove "banana"]
  hash("banana") % 8 = 33 % 8 = 1
  idx = 1
  Search bucket[1]: [("banana", 3)]
  Found! Remove entry
  buckets[1] = []
  count = 3
  loadFactor = 3/8 = 0.375
  
  Final buckets:
    [0] -> null
    [1] -> null (removed)
    [2] -> [("apple", 5), ("apricot", 2)]
    [3] -> null
    [4] -> null
    [5] -> null
    [6] -> null
    [7] -> [("grape", 7)]
2. Hash Table with Open Addressing (Linear Probing)
text
[Initialize Hash Table]
size = 5
buckets = [null, null, null, null, null]

[Put "cat", 1]
  hash("cat") = (3+1+20) % 5 = 24 % 5 = 4
  idx = 4
  buckets[4] = ("cat", 1)
  
  State: [null, null, null, null, ("cat", 1)]

[Put "dog", 2]
  hash("dog") = (4+15+7) % 5 = 26 % 5 = 1
  idx = 1
  buckets[1] = ("dog", 2)
  
  State: [null, ("dog", 2), null, null, ("cat", 1)]

[Put "bat", 3]
  hash("bat") = (2+1+20) % 5 = 23 % 5 = 3
  idx = 3
  buckets[3] = ("bat", 3)
  
  State: [null, ("dog", 2), null, ("bat", 3), ("cat", 1)]

[Put "ant", 4]
  hash("ant") = (1+14+20) % 5 = 35 % 5 = 0
  idx = 0
  buckets[0] = ("ant", 4)
  
  State: [("ant", 4), ("dog", 2), null, ("bat", 3), ("cat", 1)]

[Put "art", 5]
  hash("art") = (1+18+20) % 5 = 39 % 5 = 4
  idx = 4
  Collision! buckets[4] occupied with ("cat", 1)
  
  [Linear probe: idx = (4 + 1) % 5 = 0]
    buckets[0] occupied with ("ant", 4)
  
  [Linear probe: idx = (4 + 2) % 5 = 1]
    buckets[1] occupied with ("dog", 2)
  
  [Linear probe: idx = (4 + 3) % 5 = 2]
    buckets[2] = null (empty!)
    buckets[2] = ("art", 5)
  
  State: [("ant", 4), ("dog", 2), ("art", 5), ("bat", 3), ("cat", 1)]

[Get "art"]
  hash("art") = 39 % 5 = 4
  idx = 4
  buckets[4] = ("cat", 1)
  key "cat" != "art" (mismatch)
  
  [Linear probe: idx = (4 + 1) % 5 = 0]
    buckets[0] = ("ant", 4)
    key "ant" != "art" (mismatch)
  
  [Linear probe: idx = (4 + 2) % 5 = 1]
    buckets[1] = ("dog", 2)
    key "dog" != "art" (mismatch)
  
  [Linear probe: idx = (4 + 3) % 5 = 2]
    buckets[2] = ("art", 5)
    key "art" == "art" (match!)
    Return 5

[Remove "dog"]
  hash("dog") = 26 % 5 = 1
  idx = 1
  buckets[1] = ("dog", 2)
  Mark as TOMBSTONE
  
  State: [("ant", 4), TOMBSTONE, ("art", 5), ("bat", 3), ("cat", 1)]
3. Two-Sum with Hash Table
text
[Array] = [2, 7, 11, 15], target = 9
[Hash Map] = {}

[Iteration 1: i=0, num=2]
  complement = 9 - 2 = 7
  [7 in hash map?] No
  [Add to map] {2: 0}

[Iteration 2: i=1, num=7]
  complement = 9 - 7 = 2
  [2 in hash map?] Yes! At index 0
  [Found pair] indices [0, 1]
  Return [0, 1]

[Result] Indices [0, 1] sum to target (2 + 7 = 9) ✓
4. Frequency Counting (Top-K)
text
[Array] = [1, 1, 1, 2, 2, 3], k = 2

[Step 1: Count Frequencies]
  freqMap = {}
  
  1 → freqMap[1] = 1
  1 → freqMap[1] = 2
  1 → freqMap[1] = 3
  2 → freqMap[2] = 1
  2 → freqMap[2] = 2
  3 → freqMap[3] = 1
  
  Final: freqMap = {1: 3, 2: 2, 3: 1}

[Step 2: Min-Heap for Top-K]
  minHeap = []
  
  Process (1, 3):
    push (3, 1)
    heap = [(3, 1)]
  
  Process (2, 2):
    push (2, 2)
    heap = [(2, 2), (3, 1)]
  
  Process (3, 1):
    push (1, 3)
    heap = [(1, 3), (3, 1), (2, 2)]
    [size > k] (3 > 2)? Yes
    pop minimum (1, 3)
    heap = [(2, 2), (3, 1)]

[Step 3: Extract Top-K]
  result = []
  pop (2, 2) → result = [2]
  pop (3, 1) → result = [2, 1]

[Result] Top 2 frequent: [1, 2] with frequencies [3, 2]
5. LRU Cache Operations
text
[Initialize LRU Cache]
capacity = 2
cache = {}
order = DoublyLinkedList (LRU at tail, MRU at head)
keyToNode = {}

[Put 1, 1]
  1 not in cache
  cache size (0) < capacity (2)
  cache[1] = 1
  node = order.addFront(1)
  keyToNode[1] = node
  
  Order: 1
  Cache: {1: 1}

[Put 2, 2]
  2 not in cache
  cache size (1) < capacity (2)
  cache[2] = 2
  node = order.addFront(2)
  keyToNode[2] = node
  
  Order: 2 ↔ 1 (head ↔ tail)
  Cache: {1: 1, 2: 2}

[Get 1]
  1 in cache
  value = cache[1] = 1
  order.moveToFront(keyToNode[1])
  
  Order: 1 ↔ 2 (1 moved to head)
  
  Return 1

[Put 3, 3]
  3 not in cache
  cache size (2) >= capacity (2)
  [Evict LRU]
    lruKey = order.removeTail() = 2
    cache.remove(2)
    keyToNode.remove(2)
  
  cache[3] = 3
  node = order.addFront(3)
  keyToNode[3] = node
  
  Order: 3 ↔ 1 (2 evicted)
  Cache: {1: 1, 3: 3}

[Put 2, 2]
  2 not in cache
  cache size (2) >= capacity (2)
  [Evict LRU]
    lruKey = order.removeTail() = 1
    cache.remove(1)
    keyToNode.remove(1)
  
  cache[2] = 2
  node = order.addFront(2)
  keyToNode[2] = node
  
  Order: 2 ↔ 3
  Cache: {3: 3, 2: 2}

[Get 1]
  1 not in cache
  Return -1

[Get 3]
  3 in cache
  value = cache[3] = 3
  order.moveToFront(keyToNode[3])
  
  Order: 3 ↔ 2
  
  Return 3
6. Bloom Filter Operations
text
[Initialize Bloom Filter]
size = 10 bits
numHashFunctions = 3
bits = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]

[Add "apple"]
  hash("apple", 0) % 10 = h1 = 2
  bits[2] = 1
  
  hash("apple", 1) % 10 = h2 = 5
  bits[5] = 1
  
  hash("apple", 2) % 10 = h3 = 8
  bits[8] = 1
  
  bits = [0, 0, 1, 0, 0, 1, 0, 0, 1, 0]

[Add "banana"]
  hash("banana", 0) % 10 = h1 = 1
  bits[1] = 1
  
  hash("banana", 1) % 10 = h2 = 4
  bits[4] = 1
  
  hash("banana", 2) % 10 = h3 = 7
  bits[7] = 1
  
  bits = [0, 1, 1, 0, 1, 1, 0, 1, 1, 0]

[Contains "apple"]
  hash("apple", 0) % 10 = 2, bits[2] = 1 ✓
  hash("apple", 1) % 10 = 5, bits[5] = 1 ✓
  hash("apple", 2) % 10 = 8, bits[8] = 1 ✓
  
  Return true (definitely in set)

[Contains "grape"]
  hash("grape", 0) % 10 = 3
  bits[3] = 0 ✗
  
  Return false (definitely NOT in set)

[Contains "cherry"]
  hash("cherry", 0) % 10 = 2, bits[2] = 1 ✓
  hash("cherry", 1) % 10 = 5, bits[5] = 1 ✓
  hash("cherry", 2) % 10 = 9
  bits[9] = 0 ✗
  
  Return false (definitely NOT in set)
7. Hash Table Resizing (Detailed Trace)
text
[Initial State]
size = 4, capacity threshold = 0.75
buckets = [null, null, null, null]
count = 0

[Insert: "a"=1]
  hash("a") % 4 = 1
  buckets[1] = ("a", 1)
  count = 1, loadFactor = 1/4 = 0.25

[Insert: "b"=2]
  hash("b") % 4 = 2
  buckets[2] = ("b", 2)
  count = 2, loadFactor = 2/4 = 0.50

[Insert: "c"=3]
  hash("c") % 4 = 3
  buckets[3] = ("c", 3)
  count = 3, loadFactor = 3/4 = 0.75

[Insert: "d"=4]
  hash("d") % 4 = 0
  buckets[0] = ("d", 4)
  count = 4, loadFactor = 4/4 = 1.0 > 0.75
  
  [RESIZE TRIGGERED]
  
  oldSize = 4
  newSize = 8
  oldBuckets = [("d", 4), ("a", 1), ("b", 2), ("c", 3)]
  
  [Rehash all entries with new size 8]
  
    "a": hash("a") % 8 = 1
    buckets[1] = ("a", 1)
    
    "b": hash("b") % 8 = 2
    buckets[2] = ("b", 2)
    
    "c": hash("c") % 8 = 3
    buckets[3] = ("c", 3)
    
    "d": hash("d") % 8 = 0
    buckets[0] = ("d", 4)
  
  newBuckets = [("d", 4), ("a", 1), ("b", 2), ("c", 3), null, null, null, null]
  
  count = 4
  loadFactor = 4/8 = 0.50

[Final State]
size = 8
buckets: [("d", 4), ("a", 1), ("b", 2), ("c", 3), null, null, null, null]
loadFactor = 0.50 (well below 0.75 threshold)
8. Collision Resolution: Separate Chaining vs Linear Probing
text
[Scenario: Multiple collisions]
Insert: "ab", "ba", "aa" (all hash to index 0)

[Separate Chaining]
  buckets[0] → chain of entries
    ↓
    ("ab", 1)
    ↓
    ("ba", 2)
    ↓
    ("aa", 3)
  
  Get "ba": O(1) hash, then O(chain_length) search
  All items in single chain; chain_length = 3

[Linear Probing]
  Probe sequence: 0, 1, 2, 3, ...
  
  buckets[0] = ("ab", 1)
  buckets[1] = ("ba", 2)  ← displaced to 1
  buckets[2] = ("aa", 3)  ← displaced to 2
  
  Get "ba": O(1) hash to 0, collision detected
            O(1) hash to 1, found!
  
  Problem: Clustering (consecutive occupied slots)
9. Consistent Hashing (Node Distribution)
text
[Consistent Hash Ring (size 10)]

[Add Node A]
  hash(A_0) % 10 = 1
  hash(A_1) % 10 = 4
  hash(A_2) % 10 = 7
  
  ring: {1: A, 4: A, 7: A}
  sortedKeys: [1, 4, 7]

[Add Node B]
  hash(B_0) % 10 = 2
  hash(B_1) % 10 = 5
  hash(B_2) % 10 = 8
  
  ring: {1: A, 2: B, 4: A, 5: B, 7: A, 8: B}
  sortedKeys: [1, 2, 4, 5, 7, 8]

[Get Node for "key1"]
  hash("key1") % 10 = 3
  [Find next >= 3] → 4
  ring[4] = A
  
  Node for "key1": A

[Get Node for "key2"]
  hash("key2") % 10 = 6
  [Find next >= 6] → 7
  ring[7] = A
  
  Node for "key2": A

[Get Node for "key3"]
  hash("key3") % 10 = 9
  [Find next >= 9] → (wrap to 0) → 1
  ring[1] = A
  
  Node for "key3": A

[Add Node C]
  hash(C_0) % 10 = 3
  hash(C_1) % 10 = 6
  hash(C_2) % 10 = 9
  
  ring: {1: A, 2: B, 3: C, 4: A, 5: B, 6: C, 7: A, 8: B, 9: C}
  sortedKeys: [1, 2, 3, 4, 5, 6, 7, 8, 9]
  
  [Impact]
  "key1" (hash=3) now maps to C (was A)
  "key2" (hash=6) now maps to C (was A)
  "key3" (hash=9) now maps to C (was A)
  
  Only 3 keys remapped out of many (O(k/n) keys affected)
These interactive traces illuminate the internal mechanics of hash table operations—see how state evolves, collisions occur, data redistributes, and algorithms execute step by step. Master these patterns for hash table mastery!