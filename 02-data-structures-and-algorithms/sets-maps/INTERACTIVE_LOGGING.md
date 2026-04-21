# 🟢 Interactive Logging: Sets & Maps – Step-by-Step Operation Traces

## 1. Hash Set with Separate Chaining (Basic Operations)

```text
[Initialize Hash Set]
size = 4
buckets = [null, null, null, null]
count = 0
loadFactor = 0.0

[Add 10]
  hash(10) = 10 % 4 = 2
  idx = 2
  buckets[2] = [10]
  count = 1
  loadFactor = 1/4 = 0.25
  
  Buckets state:
    [0] -> null
    [1] -> null
    [2] -> [10]
    [3] -> null

[Add 25]
  hash(25) = 25 % 4 = 1
  idx = 1
  buckets[1] = [25]
  count = 2
  loadFactor = 2/4 = 0.50
  
  Buckets state:
    [0] -> null
    [1] -> [25]
    [2] -> [10]
    [3] -> null

[Add 10] (Duplicate)
  hash(10) = 10 % 4 = 2
  idx = 2
  Search bucket[2]: [10]
  Found 10! Element already exists
  Return false (no insertion)
  count = 2 (unchanged)

[Add 14]
  hash(14) = 14 % 4 = 2
  idx = 2
  Collision! Search bucket[2]: [10]
  14 not found; append to chain
  buckets[2] = [10, 14]
  count = 3
  loadFactor = 3/4 = 0.75
  
  Buckets state:
    [0] -> null
    [1] -> [25]
    [2] -> [10, 14]
    [3] -> null

[Add 30]
  hash(30) = 30 % 4 = 2
  idx = 2
  Collision! Search bucket[2]: [10, 14]
  30 not found; append to chain
  buckets[2] = [10, 14, 30]
  count = 4
  loadFactor = 4/4 = 1.0 > 0.75 → RESIZE
  
  [Resize triggered]
    newSize = 4 * 2 = 8
    oldBuckets = [null, [25], [10, 14, 30], null]
    
    Rehash all elements:
      hash(25) % 8 = 1
      hash(10) % 8 = 2
      hash(14) % 8 = 6
      hash(30) % 8 = 6
    
    newBuckets:
      [0] -> null
      [1] -> [25]
      [2] -> [10]
      [3] -> null
      [4] -> null
      [5] -> null
      [6] -> [14, 30]
      [7] -> null
  
  loadFactor = 4/8 = 0.50

[Contains 14]
  hash(14) % 8 = 6
  idx = 6
  Search bucket[6]: [14, 30]
  Found! Return true

[Contains 99]
  hash(99) % 8 = 3
  idx = 3
  bucket[3] = null
  Return false

[Remove 14]
  hash(14) % 8 = 6
  idx = 6
  Search bucket[6]: [14, 30]
  Found at position 0; remove
  buckets[6] = [30]
  count = 3
  loadFactor = 3/8 = 0.375
  
  Final buckets:
    [0] -> null
    [1] -> [25]
    [2] -> [10]
    [3] -> null
    [4] -> null
    [5] -> null
    [6] -> [30]
    [7] -> null
```

***

## 2. Tree Set Operations (Red-Black Tree)

```text
[Initialize Tree Set]
root = null
count = 0

[Add 50]
  root = null; insert as root
  root = Node(50, color=BLACK)
  count = 1
  
  Tree structure:
       50(B)

[Add 30]
  50 > 30; go left
  Insert as left child
  Node(30, color=RED)
  count = 2
  
  Tree structure:
       50(B)
       /
     30(R)

[Add 70]
  50 < 70; go right
  Insert as right child
  Node(70, color=RED)
  count = 3
  
  Tree structure:
       50(B)
       /   \
     30(R) 70(R)

[Add 20]
  50 > 20; go left
  30 > 20; go left
  Insert as left child of 30
  Node(20, color=RED)
  count = 4
  
  [Check balance]
    20(R) → parent 30(R) → grandparent 50(B)
    Both 20 and 30 are red (violation!)
    Uncle 70 is red
    [Recolor] 30→BLACK, 70→BLACK, 50→RED
  
  Tree structure:
       50(R)
       /   \
     30(B) 70(B)
     /
   20(R)
  
  [Fix root] root.color = BLACK
  
  Final tree:
       50(B)
       /   \
     30(B) 70(B)
     /
   20(R)

[Add 40]
  50 > 40; go left
  30 < 40; go right
  Insert as right child of 30
  Node(40, color=RED)
  count = 5
  
  [Check balance]
    40(R) → parent 30(B) → sibling 20(R)
    No violation (parent is black)
  
  Tree structure:
       50(B)
       /   \
     30(B) 70(B)
     /  \
   20(R) 40(R)

[Add 60]
  50 < 60; go right
  70 > 60; go left
  Insert as left child of 70
  Node(60, color=RED)
  count = 6
  
  Tree structure:
       50(B)
       /   \
     30(B) 70(B)
     /  \   /
   20(R) 40(R) 60(R)

[Contains 40]
  Start at root 50
  50 > 40; go left to 30
  30 < 40; go right to 40
  Found! Return true

[Contains 100]
  Start at root 50
  50 < 100; go right to 70
  70 < 100; go right
  node.right = null
  Not found! Return false

[First Element]
  Start at root 50
  Go left repeatedly:
    50 → 30 → 20 → null
  Leftmost node: 20
  Return 20

[Last Element]
  Start at root 50
  Go right repeatedly:
    50 → 70 → null
  Rightmost node: 70
  Return 70

[Higher(40)]
  Find smallest element > 40
  Start at root 50
  50 > 40; potential result=50, go left
  30 < 40; go right
  40 = 40; go right
  node.right = null
  Return 50

[Remove 30]
  Find node 30
  Node has two children (20 and 40)
  [Find successor] Leftmost in right subtree: 40
  Replace 30's value with 40
  Delete node 40 (now a leaf)
  count = 5
  
  Tree structure:
       50(B)
       /   \
     40(B) 70(B)
     /      /
   20(R)  60(R)
```

***

## 3. Hash Map Operations

```text
[Initialize Hash Map]
size = 4
buckets = [null, null, null, null]
count = 0

[Put "apple", 5]
  hash("apple") = 50 % 4 = 2
  idx = 2
  buckets[2] = [("apple", 5)]
  count = 1
  
  Buckets state:
    [0] -> null
    [1] -> null
    [2] -> [("apple", 5)]
    [3] -> null

[Put "banana", 3]
  hash("banana") = 33 % 4 = 1
  idx = 1
  buckets[1] = [("banana", 3)]
  count = 2
  
  Buckets state:
    [0] -> null
    [1] -> [("banana", 3)]
    [2] -> [("apple", 5)]
    [3] -> null

[Put "apple", 10] (Update)
  hash("apple") = 50 % 4 = 2
  idx = 2
  Search bucket[2]: [("apple", 5)]
  Found "apple"! Update value: 5 → 10
  buckets[2] = [("apple", 10)]
  count = 2 (unchanged)
  Return old value: 5

[Get "apple"]
  hash("apple") = 50 % 4 = 2
  idx = 2
  Search bucket[2]: [("apple", 10)]
  Found! Return 10

[ContainsKey "banana"]
  hash("banana") = 33 % 4 = 1
  idx = 1
  Search bucket[1]: [("banana", 3)]
  Found! Return true

[ContainsValue 3]
  Scan all buckets:
    [0] -> null
    [1] -> [("banana", 3)] ✓ value 3 found!
  Return true

[Remove "banana"]
  hash("banana") = 33 % 4 = 1
  idx = 1
  Search bucket[1]: [("banana", 3)]
  Found! Remove entry
  buckets[1] = []
  count = 1
  Return 3 (removed value)
  
  Final buckets:
    [0] -> null
    [1] -> null
    [2] -> [("apple", 10)]
    [3] -> null
```

***

## 4. Set Operations (Union, Intersection, Difference)

```text
[Set A] = {1, 2, 3, 4, 5}
[Set B] = {4, 5, 6, 7}

[Union A ∪ B]
  result = {}
  
  Add all from A:
    result.add(1) → {1}
    result.add(2) → {1, 2}
    result.add(3) → {1, 2, 3}
    result.add(4) → {1, 2, 3, 4}
    result.add(5) → {1, 2, 3, 4, 5}
  
  Add all from B:
    result.add(4) → already exists, skip
    result.add(5) → already exists, skip
    result.add(6) → {1, 2, 3, 4, 5, 6}
    result.add(7) → {1, 2, 3, 4, 5, 6, 7}
  
  Result: {1, 2, 3, 4, 5, 6, 7}

[Intersection A ∩ B]
  result = {}
  
  For each in A, check if in B:
    1 in B? No
    2 in B? No
    3 in B? No
    4 in B? Yes → result.add(4) → {4}
    5 in B? Yes → result.add(5) → {4, 5}
  
  Result: {4, 5}

[Difference A - B]
  result = {}
  
  For each in A, check if NOT in B:
    1 not in B? Yes → result.add(1) → {1}
    2 not in B? Yes → result.add(2) → {1, 2}
    3 not in B? Yes → result.add(3) → {1, 2, 3}
    4 not in B? No (4 in B)
    5 not in B? No (5 in B)
  
  Result: {1, 2, 3}

[Symmetric Difference A Δ B]
  result = (A - B) ∪ (B - A)
  
  A - B = {1, 2, 3}
  B - A:
    4 not in A? No (4 in A)
    5 not in A? No (5 in A)
    6 not in A? Yes → {6}
    7 not in A? Yes → {6, 7}
  B - A = {6, 7}
  
  Union: {1, 2, 3} ∪ {6, 7} = {1, 2, 3, 6, 7}
  
  Result: {1, 2, 3, 6, 7}
```

***

## 5. Two Sum with Hash Map

```text
[Array] = [2, 7, 11, 15], target = 9
[Hash Map] = {}

[Iteration 0: num=2]
  complement = 9 - 2 = 7
  [7 in map?] No
  [Add to map] {2: 0}
  
  Map state: {2: 0}

[Iteration 1: num=7]
  complement = 9 - 7 = 2
  [2 in map?] Yes! At index 0
  [Found pair] indices [0, 1]
  
  Return [0, 1]

[Result] Indices [0, 1] sum to target (2 + 7 = 9) ✓
```

***

## 6. Group Anagrams

```text
[Words] = ["eat", "tea", "tan", "ate", "nat", "bat"]
[Anagram Map] = {}

[Process "eat"]
  sorted = "aet"
  map["aet"] = ["eat"]
  
  Map: {"aet": ["eat"]}

[Process "tea"]
  sorted = "aet"
  map["aet"].append("tea")
  
  Map: {"aet": ["eat", "tea"]}

[Process "tan"]
  sorted = "ant"
  map["ant"] = ["tan"]
  
  Map: {"aet": ["eat", "tea"], "ant": ["tan"]}

[Process "ate"]
  sorted = "aet"
  map["aet"].append("ate")
  
  Map: {"aet": ["eat", "tea", "ate"], "ant": ["tan"]}

[Process "nat"]
  sorted = "ant"
  map["ant"].append("nat")
  
  Map: {"aet": ["eat", "tea", "ate"], "ant": ["tan", "nat"]}

[Process "bat"]
  sorted = "abt"
  map["abt"] = ["bat"]
  
  Final Map: {
    "aet": ["eat", "tea", "ate"],
    "ant": ["tan", "nat"],
    "abt": ["bat"]
  }

[Extract groups]
  result = [
    ["eat", "tea", "ate"],
    ["tan", "nat"],
    ["bat"]
  ]
```

***

## 7. Longest Consecutive Sequence

```text
[Array] = [100, 4, 200, 1, 3, 2]
[Set] = {100, 4, 200, 1, 3, 2}
maxLength = 0

[Check 100]
  99 in set? No (sequence start)
  [Build sequence]
    100 in set? Yes, length = 1
    101 in set? No
  currentLength = 1
  maxLength = max(0, 1) = 1

[Check 4]
  3 in set? Yes (NOT sequence start, skip)

[Check 200]
  199 in set? No (sequence start)
  [Build sequence]
    200 in set? Yes, length = 1
    201 in set? No
  currentLength = 1
  maxLength = max(1, 1) = 1

[Check 1]
  0 in set? No (sequence start)
  [Build sequence]
    1 in set? Yes, length = 1
    2 in set? Yes, length = 2
    3 in set? Yes, length = 3
    4 in set? Yes, length = 4
    5 in set? No
  currentLength = 4
  maxLength = max(1, 4) = 4

[Check 3]
  2 in set? Yes (NOT sequence start, skip)

[Check 2]
  1 in set? Yes (NOT sequence start, skip)

[Result] Longest consecutive sequence length: 4 (1→2→3→4)
```

***

## 8. LRU Cache Operations

```text
[Initialize LRU Cache]
capacity = 2
cache = {}
order = DoublyLinkedList (LRU at tail, MRU at head)

[Put 1, 1]
  1 not in cache
  cache.size (0) < capacity (2)
  cache[1] = 1
  order.addToFront(1)
  
  Order: 1 (MRU)
  Cache: {1: 1}

[Put 2, 2]
  2 not in cache
  cache.size (1) < capacity (2)
  cache[2] = 2
  order.addToFront(2)
  
  Order: 2 ↔ 1 (MRU ↔ LRU)
  Cache: {1: 1, 2: 2}

[Get 1]
  1 in cache
  value = cache[1] = 1
  order.moveToFront(1)
  
  Order: 1 ↔ 2 (1 moved to MRU)
  
  Return 1

[Put 3, 3]
  3 not in cache
  cache.size (2) >= capacity (2)
  [Evict LRU]
    lruKey = order.removeTail() = 2
    cache.remove(2)
  
  cache[3] = 3
  order.addToFront(3)
  
  Order: 3 ↔ 1 (2 evicted)
  Cache: {1: 1, 3: 3}

[Get 2]
  2 not in cache
  Return -1

[Put 4, 4]
  4 not in cache
  cache.size (2) >= capacity (2)
  [Evict LRU]
    lruKey = order.removeTail() = 1
    cache.remove(1)
  
  cache[4] = 4
  order.addToFront(4)
  
  Order: 4 ↔ 3
  Cache: {3: 3, 4: 4}

[Get 1]
  1 not in cache
  Return -1

[Get 3]
  3 in cache
  value = cache[3] = 3
  order.moveToFront(3)
  
  Order: 3 ↔ 4
  
  Return 3

[Get 4]
  4 in cache
  value = cache[4] = 4
  order.moveToFront(4)
  
  Order: 4 ↔ 3
  
  Return 4
```

***

## 9. Multiset (Frequency Counting)

```text
[Initialize Multiset]
freqMap = {}
totalCount = 0

[Add "apple"]
  freqMap["apple"] = 0 + 1 = 1
  totalCount = 1
  
  Map: {"apple": 1}

[Add "banana"]
  freqMap["banana"] = 0 + 1 = 1
  totalCount = 2
  
  Map: {"apple": 1, "banana": 1}

[Add "apple"]
  freqMap["apple"] = 1 + 1 = 2
  totalCount = 3
  
  Map: {"apple": 2, "banana": 1}

[Add "apple"]
  freqMap["apple"] = 2 + 1 = 3
  totalCount = 4
  
  Map: {"apple": 3, "banana": 1}

[Count "apple"]
  freqMap["apple"] = 3
  Return 3

[Count "cherry"]
  "cherry" not in freqMap
  Return 0

[Remove "apple"]
  freqMap["apple"] = 3 - 1 = 2
  totalCount = 3
  
  Map: {"apple": 2, "banana": 1}

[Remove "apple"]
  freqMap["apple"] = 2 - 1 = 1
  totalCount = 2
  
  Map: {"apple": 1, "banana": 1}

[Remove "apple"]
  freqMap["apple"] = 1 - 1 = 0
  Remove "apple" from map
  totalCount = 1
  
  Map: {"banana": 1}

[Size]
  totalCount = 1

[UniqueElements]
  freqMap.size() = 1
```

***

## 10. Bloom Filter Operations

```text
[Initialize Bloom Filter]
size = 10 bits
numHashFunctions = 3
bits = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]

[Add "apple"]
  hash("apple", 0) % 10 = 2
  bits[2] = 1
  
  hash("apple", 1) % 10 = 5
  bits[5] = 1
  
  hash("apple", 2) % 10 = 8
  bits[8] = 1
  
  bits = [0, 0, 1, 0, 0, 1, 0, 0, 1, 0]

[Add "banana"]
  hash("banana", 0) % 10 = 1
  bits[1] = 1
  
  hash("banana", 1) % 10 = 4
  bits[4] = 1
  
  hash("banana", 2) % 10 = 7
  bits[7] = 1
  
  bits = [0, 1, 1, 0, 1, 1, 0, 1, 1, 0]

[Add "cherry"]
  hash("cherry", 0) % 10 = 2
  bits[2] = 1 (already set)
  
  hash("cherry", 1) % 10 = 6
  bits[6] = 1
  
  hash("cherry", 2) % 10 = 9
  bits[9] = 1
  
  bits = [0, 1, 1, 0, 1, 1, 1, 1, 1, 1]

[Contains "apple"]
  hash("apple", 0) % 10 = 2, bits[2] = 1 ✓
  hash("apple", 1) % 10 = 5, bits[5] = 1 ✓
  hash("apple", 2) % 10 = 8, bits[8] = 1 ✓
  
  All bits set! Return true (probably in set)

[Contains "grape"]
  hash("grape", 0) % 10 = 3
  bits[3] = 0 ✗
  
  At least one bit not set! Return false (definitely NOT in set)

[Contains "melon"]
  hash("melon", 0) % 10 = 1, bits[1] = 1 ✓
  hash("melon", 1) % 10 = 2, bits[2] = 1 ✓
  hash("melon", 2) % 10 = 4, bits[4] = 1 ✓
  
  All bits set! Return true (FALSE POSITIVE - "melon" was never added!)
  
  Note: Bits 1, 2, 4 were set by other elements
```

***

## 11. Consistent Hashing (Node Distribution)

```text
[Consistent Hash Ring (size 10)]

[Add Node A with 3 replicas]
  hash("A_0") % 10 = 1
  ring[1] = A
  
  hash("A_1") % 10 = 4
  ring[4] = A
  
  hash("A_2") % 10 = 7
  ring[7] = A
  
  ring: {1: A, 4: A, 7: A}
  sortedKeys: [1, 4, 7]

[Add Node B with 3 replicas]
  hash("B_0") % 10 = 2
  ring[2] = B
  
  hash("B_1") % 10 = 5
  ring[5] = B
  
  hash("B_2") % 10 = 8
  ring[8] = B
  
  ring: {1: A, 2: B, 4: A, 5: B, 7: A, 8: B}
  sortedKeys: [1, 2, 4, 5, 7, 8]

[Get Node for "key1"]
  hash("key1") % 10 = 3
  [Find next >= 3 on ring] → 4
  ring[4] = A
  
  Node for "key1": A

[Get Node for "key2"]
  hash("key2") % 10 = 6
  [Find next >= 6 on ring] → 7
  ring[7] = A
  
  Node for "key2": A

[Get Node for "key3"]
  hash("key3") % 10 = 9
  [Find next >= 9 on ring] → (wrap to 0) → 1
  ring[1] = A
  
  Node for "key3": A

[Add Node C with 3 replicas]
  hash("C_0") % 10 = 3
  ring[3] = C
  
  hash("C_1") % 10 = 6
  ring[6] = C
  
  hash("C_2") % 10 = 9
  ring[9] = C
  
  ring: {1: A, 2: B, 3: C, 4: A, 5: B, 6: C, 7: A, 8: B, 9: C}
  sortedKeys: [1, 2, 3, 4, 5, 6, 7, 8, 9]

[Re-check keys after adding C]
  "key1" (hash=3):
    [Find next >= 3] → 3
    ring[3] = C
    Node for "key1": C (migrated from A)
  
  "key2" (hash=6):
    [Find next >= 6] → 6
    ring[6] = C
    Node for "key2": C (migrated from A)
  
  "key3" (hash=9):
    [Find next >= 9] → 9
    ring[9] = C
    Node for "key3": C (migrated from A)

[Impact]
  Only keys hashing to positions now occupied by C are remapped
  O(k/n) keys affected where k = total keys, n = nodes
```

***

## 12. Disjoint Set (Union-Find)

```text
[Initialize Disjoint Set]
parent = {}
rank = {}

[MakeSet for 0, 1, 2, 3, 4]
  parent[0] = 0, rank[0] = 0
  parent[1] = 1, rank[1] = 0
  parent[2] = 2, rank[2] = 0
  parent[3] = 3, rank[3] = 0
  parent[4] = 4, rank[4] = 0
  
  Forest: {0}, {1}, {2}, {3}, {4}

[Union(0, 1)]
  root0 = find(0) = 0
  root1 = find(1) = 1
  
  rank[0] = 0, rank[1] = 0 (equal)
  parent[1] = 0
  rank[0] = 1
  
  Forest: {0, 1}, {2}, {3}, {4}
  
  Tree:
    0 (rank=1)
    └─ 1

[Union(2, 3)]
  root2 = find(2) = 2
  root3 = find(3) = 3
  
  rank[2] = 0, rank[3] = 0 (equal)
  parent[3] = 2
  rank[2] = 1
  
  Forest: {0, 1}, {2, 3}, {4}
  
  Tree:
    2 (rank=1)
    └─ 3

[Union(0, 2)]
  root0 = find(0) = 0
  root2 = find(2) = 2
  
  rank[0] = 1, rank[2] = 1 (equal)
  parent[2] = 0
  rank[0] = 2
  
  Forest: {0, 1, 2, 3}, {4}
  
  Tree:
    0 (rank=2)
    ├─ 1
    └─ 2
       └─ 3

[Find(3) with path compression]
  parent[3] = 2
  parent[2] = 0
  parent[0] = 0 (root)
  
  [Path compression]
    parent[3] = 0
    parent[2] = 0 (already optimized)
  
  Tree after compression:
    0 (rank=2)
    ├─ 1
    ├─ 2
    └─ 3

[IsConnected(1, 3)]
  find(1) = 0
  find(3) = 0
  0 == 0 → true

[IsConnected(1, 4)]
  find(1) = 0
  find(4) = 4
  0 != 4 → false

[Union(3, 4)]
  root3 = find(3) = 0
  root4 = find(4) = 4
  
  rank[0] = 2, rank[4] = 0
  rank[0] > rank[4]
  parent[4] = 0
  
  Forest: {0, 1, 2, 3, 4}
  
  Final tree:
    0 (rank=2)
    ├─ 1
    ├─ 2
    ├─ 3
    └─ 4

[Count Components]
  roots = set()
  for each element:
    roots.add(find(element))
  
  find(0) = 0 → roots = {0}
  find(1) = 0 → roots = {0}
  find(2) = 0 → roots = {0}
  find(3) = 0 → roots = {0}
  find(4) = 0 → roots = {0}
  
  Number of components: 1
```

***

## 13. Subarray Sum Equals K (Prefix Sum Map)

```text
[Array] = [1, 1, 1], k = 2
[Prefix Sum Map] = {0: 1}
currentSum = 0
count = 0

[Iteration 0: num=1]
  currentSum = 0 + 1 = 1
  
  [Check if (currentSum - k) in map]
    1 - 2 = -1
    -1 in map? No
  
  [Add currentSum to map]
    map[1] = 0 + 1 = 1
  
  Map: {0: 1, 1: 1}
  count = 0

[Iteration 1: num=1]
  currentSum = 1 + 1 = 2
  
  [Check if (currentSum - k) in map]
    2 - 2 = 0
    0 in map? Yes! count += map[0] = 1
  
  count = 1
  
  [Add currentSum to map]
    map[2] = 0 + 1 = 1
  
  Map: {0: 1, 1: 1, 2: 1}

[Iteration 2: num=1]
  currentSum = 2 + 1 = 3
  
  [Check if (currentSum - k) in map]
    3 - 2 = 1
    1 in map? Yes! count += map[1] = 1
  
  count = 2
  
  [Add currentSum to map]
    map[3] = 0 + 1 = 1
  
  Map: {0: 1, 1: 1, 2: 1, 3: 1}

[Result] Number of subarrays with sum = 2: 2
  Subarrays: [1,1] (indices 0-1), [1,1] (indices 1-2)
```

***

## 14. Valid Sudoku (Multiple Sets)

```text
[Board] (simplified 4x4 for demonstration)
  1 2 . .
  3 4 . .
  . . 1 2
  . . 3 4

[Initialize]
  rows = [set(), set(), set(), set()]
  cols = [set(), set(), set(), set()]
  boxes = [set(), set(), set(), set()]

[Check (0, 0) = 1]
  num = 1
  boxIdx = (0/2)*2 + (0/2) = 0
  
  1 in rows[0]? No
  1 in cols[0]? No
  1 in boxes[0]? No
  
  rows[0].add(1) → rows[0] = {1}
  cols[0].add(1) → cols[0] = {1}
  boxes[0].add(1) → boxes[0] = {1}

[Check (0, 1) = 2]
  num = 2
  boxIdx = (0/2)*2 + (1/2) = 0
  
  2 in rows[0]? No
  2 in cols[1]? No
  2 in boxes[0]? No
  
  rows[0].add(2) → rows[0] = {1, 2}
  cols[1].add(2) → cols[1] = {2}
  boxes[0].add(2) → boxes[0] = {1, 2}

[Check (1, 0) = 3]
  num = 3
  boxIdx = (1/2)*2 + (0/2) = 0
  
  3 in rows[1]? No
  3 in cols[0]? No
  3 in boxes[0]? No
  
  rows[1].add(3) → rows[1] = {3}
  cols[0].add(3) → cols[0] = {1, 3}
  boxes[0].add(3) → boxes[0] = {1, 2, 3}

[Check (1, 1) = 4]
  num = 4
  boxIdx = (1/2)*2 + (1/2) = 0
  
  4 in rows[1]? No
  4 in cols[1]? No
  4 in boxes[0]? No
  
  rows[1].add(4) → rows[1] = {3, 4}
  cols[1].add(4) → cols[1] = {2, 4}
  boxes[0].add(4) → boxes[0] = {1, 2, 3, 4}

[Check (2, 2) = 1]
  num = 1
  boxIdx = (2/2)*2 + (2/2) = 3
  
  1 in rows[2]? No
  1 in cols[2]? No
  1 in boxes[3]? No
  
  rows[2].add(1) → rows[2] = {1}
  cols[2].add(1) → cols[2] = {1}
  boxes[3].add(1) → boxes[3] = {1}

[Continue for remaining cells...]

[Result] Valid Sudoku: true (no duplicates detected)
```

***

These interactive traces illuminate the internal mechanics of set and map operations—watch how state evolves, collisions are handled, trees balance, and algorithms execute step by step for complete mastery![1][2][3][4][5]

[1](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/87948039/98310c60-039c-4da2-a6b6-4031351a428c/INTERACTIVE_LOGGING.md)
[2](https://www.geeksforgeeks.org/introduction-to-set-data-structure/)
[3](https://www.geeksforgeeks.org/cpp/set-in-cpp-stl/)
[4](https://www.geeksforgeeks.org/java/treeset-in-java-with-examples/)
[5](https://www.math.md/files/csjm/v30-n1/v30-n1-(pp28-48).pdf)
[6](https://www.w3schools.com/dsa/dsa_intro.php)
[7](https://www.tutorialspoint.com/data_structures_algorithms/index.htm)
[8](https://www.vibrantpublishers.com/blogs/blogs-on-programming/data-structures-for-sets)
[9](https://www.shobhituniversity.ac.in/pdf/econtent/Data-Structure-using-C-Rajesh-Pandey.pdf)
[10](https://docs.oracle.com/javase/8/docs/api/java/util/TreeSet.html)
[11](https://visualgo.net)