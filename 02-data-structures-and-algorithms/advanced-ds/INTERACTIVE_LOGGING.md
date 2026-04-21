# 🔥 Interactive Logging: Advanced Data Structures - Step-by-Step Execution & Debugging

This guide provides detailed execution traces, visual logging, and debugging techniques for advanced data structures to help you understand their internal workings and troubleshoot issues.[1][4][5]

***

## 1. Segment Tree Interactive Logging

### Build Operation - Step by Step

**Input Array:** `[1][3][5][7][9][11]`

```text
=== SEGMENT TREE BUILD LOG ===

Initial Array: [1, 3, 5, 7, 9, 11]
Tree Size: 4 * 6 = 24 nodes allocated

Step 1: Build(node=1, L=0, R=5)
  └─ Range [0,5]: Processing full array
  └─ Mid = (0+5)/2 = 2

Step 2: Build(node=2, L=0, R=2) [Left child of node 1]
  └─ Range [0,2]: [1, 3, 5]
  └─ Mid = (0+2)/2 = 1
  
Step 3: Build(node=4, L=0, R=1) [Left child of node 2]
  └─ Range [0,1]: [1, 3]
  └─ Mid = (0+1)/2 = 0

Step 4: Build(node=8, L=0, R=0) [Left child of node 4]
  └─ LEAF NODE: tree[8] = arr[0] = 1
  └─ ✓ tree[8] = 1

Step 5: Build(node=9, L=1, R=1) [Right child of node 4]
  └─ LEAF NODE: tree[9] = arr[1] = 3
  └─ ✓ tree[9] = 3

Step 6: Merge at node=4
  └─ tree[4] = tree[8] + tree[9] = 1 + 3 = 4
  └─ ✓ tree[4] = 4

Step 7: Build(node=5, L=2, R=2) [Right child of node 2]
  └─ LEAF NODE: tree[5] = arr[2] = 5
  └─ ✓ tree[5] = 5

Step 8: Merge at node=2
  └─ tree[2] = tree[4] + tree[5] = 4 + 5 = 9
  └─ ✓ tree[2] = 9

Step 9: Build(node=3, L=3, R=5) [Right child of node 1]
  └─ Range [3,5]: [7, 9, 11]
  └─ Mid = (3+5)/2 = 4

Step 10: Build(node=6, L=3, R=4) [Left child of node 3]
  └─ Range [3,4]: [7, 9]
  └─ Mid = (3+4)/2 = 3

Step 11: Build(node=12, L=3, R=3) [Left child of node 6]
  └─ LEAF NODE: tree[12] = arr[3] = 7
  └─ ✓ tree[12] = 7

Step 12: Build(node=13, L=4, R=4) [Right child of node 6]
  └─ LEAF NODE: tree[13] = arr[4] = 9
  └─ ✓ tree[13] = 9

Step 13: Merge at node=6
  └─ tree[6] = tree[12] + tree[13] = 7 + 9 = 16
  └─ ✓ tree[6] = 16

Step 14: Build(node=7, L=5, R=5) [Right child of node 3]
  └─ LEAF NODE: tree[7] = arr[5] = 11
  └─ ✓ tree[7] = 11

Step 15: Merge at node=3
  └─ tree[3] = tree[6] + tree[7] = 16 + 11 = 27
  └─ ✓ tree[3] = 27

Step 16: Merge at node=1 (ROOT)
  └─ tree[1] = tree[2] + tree[3] = 9 + 27 = 36
  └─ ✓ tree[1] = 36

=== FINAL TREE STRUCTURE ===
                    36[0-5]
                   /        \
              9[0-2]          27[3-5]
             /      \         /       \
         4[0-1]   5[2]    16[3-4]   11[5]
         /    \           /     \
      1[0]  3[1]       7[3]   9[4]

Tree Array Representation:
Index:  [0] [1] [2] [3] [4] [5] [6] [7] [8] [9][10][11][12][13]
Value:  [0][36] [9][27] [4] [5][16][11] [1] [3] [0] [0] [7] [9]
```

***

### Range Query Operation - Detailed Trace

**Query:** `sum(1, 4)` - Find sum of elements from index 1 to 4

```text
=== RANGE QUERY LOG: query(1, 4) ===

Current State:
  Array: [1, 3, 5, 7, 9, 11]
  Query Range: [1, 4]

Call 1: query(node=1, treeL=0, treeR=5, queryL=1, queryR=4)
  ├─ Current Segment: [0, 5] (covers indices 0-5)
  ├─ Query Segment: [1, 4]
  ├─ Overlap Type: PARTIAL
  │  └─ Not NO_OVERLAP (0 ≤ 4 and 5 ≥ 1)
  │  └─ Not COMPLETE_OVERLAP (1 > 0 or 4 < 5)
  ├─ Mid = (0+5)/2 = 2
  └─ Split into left[0,2] and right[3,5]

Call 2: query(node=2, treeL=0, treeR=2, queryL=1, queryR=4)
  ├─ Current Segment: [0, 2]
  ├─ Query Segment: [1, 4]
  ├─ Overlap Type: PARTIAL
  ├─ Mid = (0+2)/2 = 1
  └─ Split into left[0,1] and right[2,2]

Call 3: query(node=4, treeL=0, treeR=1, queryL=1, queryR=4)
  ├─ Current Segment: [0, 1]
  ├─ Query Segment: [1, 4]
  ├─ Overlap Type: PARTIAL
  ├─ Mid = (0+1)/2 = 0
  └─ Split into left[0,0] and right[1,1]

Call 4: query(node=8, treeL=0, treeR=0, queryL=1, queryR=4)
  ├─ Current Segment: [0, 0]
  ├─ Query Segment: [1, 4]
  ├─ Overlap Type: NO_OVERLAP (0 < 1)
  └─ ✗ Return 0

Call 5: query(node=9, treeL=1, treeR=1, queryL=1, queryR=4)
  ├─ Current Segment: [1, 1]
  ├─ Query Segment: [1, 4]
  ├─ Overlap Type: COMPLETE_OVERLAP
  │  └─ 1 ≤ 1 and 1 ≤ 4 ✓
  ├─ ✓ Return tree[9] = 3
  └─ [Collected: 3]

Back to Call 3:
  └─ leftSum = 0, rightSum = 3
  └─ Return 0 + 3 = 3

Call 6: query(node=5, treeL=2, treeR=2, queryL=1, queryR=4)
  ├─ Current Segment: [2, 2]
  ├─ Query Segment: [1, 4]
  ├─ Overlap Type: COMPLETE_OVERLAP
  ├─ ✓ Return tree[5] = 5
  └─ [Collected: 5]

Back to Call 2:
  └─ leftSum = 3, rightSum = 5
  └─ Return 3 + 5 = 8

Call 7: query(node=3, treeL=3, treeR=5, queryL=1, queryR=4)
  ├─ Current Segment: [3, 5]
  ├─ Query Segment: [1, 4]
  ├─ Overlap Type: PARTIAL
  ├─ Mid = (3+5)/2 = 4
  └─ Split into left[3,4] and right[5,5]

Call 8: query(node=6, treeL=3, treeR=4, queryL=1, queryR=4)
  ├─ Current Segment: [3, 4]
  ├─ Query Segment: [1, 4]
  ├─ Overlap Type: COMPLETE_OVERLAP
  │  └─ 1 ≤ 3 and 4 ≤ 4 ✓
  ├─ ✓ Return tree[6] = 16
  └─ [Collected: 16]

Call 9: query(node=7, treeL=5, treeR=5, queryL=1, queryR=4)
  ├─ Current Segment: [5, 5]
  ├─ Query Segment: [1, 4]
  ├─ Overlap Type: NO_OVERLAP (5 > 4)
  └─ ✗ Return 0

Back to Call 7:
  └─ leftSum = 16, rightSum = 0
  └─ Return 16 + 0 = 16

Back to Call 1:
  └─ leftSum = 8, rightSum = 16
  └─ FINAL RESULT = 8 + 16 = 24

=== QUERY RESULT ===
sum(1, 4) = 3 + 5 + 7 + 9 = 24 ✓

Nodes Visited: [1, 2, 4, 8, 9, 5, 3, 6, 7] = 9 nodes
Efficiency: O(log n) = O(log 6) ≈ 2.58 < 9 (overhead from visualization)
```

***

### Point Update Operation - Interactive Trace

**Update:** `update(index=2, newValue=8)` - Change arr from 5 to 8[1]

```text
=== POINT UPDATE LOG: update(2, 8) ===

Before Update:
  Array: [1, 3, 5, 7, 9, 11]
  Tree[1] = 36 (root sum)

Step 1: update(node=1, treeL=0, treeR=5, index=2, value=8)
  ├─ Current Node: 1 (covers [0,5])
  ├─ Target Index: 2
  ├─ Not a leaf node (0 ≠ 5)
  ├─ Mid = 2
  ├─ index(2) ≤ mid(2) → Go LEFT
  └─ Call update(node=2, ...)

Step 2: update(node=2, treeL=0, treeR=2, index=2, value=8)
  ├─ Current Node: 2 (covers [0,2])
  ├─ Target Index: 2
  ├─ Not a leaf node (0 ≠ 2)
  ├─ Mid = 1
  ├─ index(2) > mid(1) → Go RIGHT
  └─ Call update(node=5, ...)

Step 3: update(node=5, treeL=2, treeR=2, index=2, value=8)
  ├─ Current Node: 5 (covers [2,2])
  ├─ Target Index: 2
  ├─ LEAF NODE FOUND! (2 == 2)
  ├─ Old Value: tree[5] = 5
  ├─ New Value: tree[5] = 8
  └─ ✓ UPDATE: tree[5] = 5 → 8

Back to Step 2 (node=2):
  ├─ Recalculate after child update
  ├─ tree[2] = tree[4] + tree[5]
  ├─ tree[2] = 4 + 8 = 12
  └─ ✓ UPDATE: tree[2] = 9 → 12

Back to Step 1 (node=1):
  ├─ Recalculate after child update
  ├─ tree[1] = tree[2] + tree[3]
  ├─ tree[1] = 12 + 27 = 39
  └─ ✓ UPDATE: tree[1] = 36 → 39

=== UPDATE SUMMARY ===
Nodes Modified: 3 (nodes 5, 2, 1)
Path: 1 → 2 → 5
Depth: 3 levels (log₂ 6 ≈ 2.58)

After Update:
  Array: [1, 3, 8, 7, 9, 11]  (changed arr[2]: 5→8)
  Tree[1] = 39 (increased by 3)

Tree Changes:
  tree[5]: 5 → 8   (+3)
  tree[2]: 9 → 12  (+3)
  tree[1]: 36 → 39 (+3)

Visual Update Path:
                    36→39[0-5]
                   /          \
              9→12[0-2]        27[3-5]
             /        \        /      \
         4[0-1]    5→8[2]  16[3-4]  11[5]
```

***

## 2. Fenwick Tree (BIT) Interactive Logging

### Build Operation with Bit Manipulation

**Input Array:** `[3, 2, -1, 6, 5, 4, -3, 3]`

```text
=== FENWICK TREE BUILD LOG ===

Array (0-indexed): [3, 2, -1, 6, 5, 4, -3, 3]
BIT (1-indexed):   [0, ?, ?, ?, ?, ?, ?, ?, ?]

Building BIT using optimized O(n) method...

Step 1: Copy array to BIT (shift to 1-indexed)
  BIT[1] = arr[0] = 3
  BIT[2] = arr[1] = 2
  BIT[3] = arr[2] = -1
  BIT[4] = arr[3] = 6
  BIT[5] = arr[4] = 5
  BIT[6] = arr[5] = 4
  BIT[7] = arr[6] = -3
  BIT[8] = arr[7] = 3

BIT: [0, 3, 2, -1, 6, 5, 4, -3, 3]

Step 2: Build tree bottom-up

i=1 (binary: 0001):
  ├─ BIT[1] = 3
  ├─ parent = 1 + (1 & -1) = 1 + 1 = 2
  ├─ BIT[2] += BIT[1]
  └─ BIT[2] = 2 + 3 = 5

i=2 (binary: 0010):
  ├─ BIT[2] = 5
  ├─ parent = 2 + (2 & -2) = 2 + 2 = 4
  ├─ BIT[4] += BIT[2]
  └─ BIT[4] = 6 + 5 = 11

i=3 (binary: 0011):
  ├─ BIT[3] = -1
  ├─ parent = 3 + (3 & -3) = 3 + 1 = 4
  ├─ BIT[4] += BIT[3]
  └─ BIT[4] = 11 + (-1) = 10

i=4 (binary: 0100):
  ├─ BIT[4] = 10
  ├─ parent = 4 + (4 & -4) = 4 + 4 = 8
  ├─ BIT[8] += BIT[4]
  └─ BIT[8] = 3 + 10 = 13

i=5 (binary: 0101):
  ├─ BIT[5] = 5
  ├─ parent = 5 + (5 & -5) = 5 + 1 = 6
  ├─ BIT[6] += BIT[5]
  └─ BIT[6] = 4 + 5 = 9

i=6 (binary: 0110):
  ├─ BIT[6] = 9
  ├─ parent = 6 + (6 & -6) = 6 + 2 = 8
  ├─ BIT[8] += BIT[6]
  └─ BIT[8] = 13 + 9 = 22

i=7 (binary: 0111):
  ├─ BIT[7] = -3
  ├─ parent = 7 + (7 & -7) = 7 + 1 = 8
  ├─ BIT[8] += BIT[7]
  └─ BIT[8] = 22 + (-3) = 19

i=8 (binary: 1000):
  ├─ BIT[8] = 19
  ├─ parent = 8 + (8 & -8) = 8 + 8 = 16 > n
  └─ Skip (out of bounds)

=== FINAL BIT STRUCTURE ===
Index:  [0] [1] [2] [3] [4] [5] [6] [7] [8]
Value:  [0] [3] [5][-1][10] [5] [9][-3][19]

Responsibility Ranges:
  BIT[1]: sum(arr[0..0])    = 3
  BIT[2]: sum(arr[0..1])    = 5
  BIT[3]: sum(arr[2..2])    = -1
  BIT[4]: sum(arr[0..3])    = 10
  BIT[5]: sum(arr[4..4])    = 5
  BIT[6]: sum(arr[4..5])    = 9
  BIT[7]: sum(arr[6..6])    = -3
  BIT[8]: sum(arr[0..7])    = 19
```

***

### Prefix Sum Query with Bit Manipulation

**Query:** `prefixSum(5)` - Sum of arr[0..5]

```text
=== PREFIX SUM QUERY: prefixSum(5) ===

Query: Sum of elements from index 0 to 5
Target: arr[0] + arr[1] + ... + arr[5]
Expected: 3 + 2 + (-1) + 6 + 5 + 4 = 19

Convert to 1-indexed: index = 5 + 1 = 6

Step 1: index = 6 (binary: 0110)
  ├─ sum = 0
  ├─ sum += BIT[6] = 0 + 9 = 9
  ├─ Remove LSB: 6 & -6 = 0110 & 1010 = 0010 = 2
  ├─ index = 6 - 2 = 4
  └─ Current sum = 9 (represents arr[4..5])

Step 2: index = 4 (binary: 0100)
  ├─ sum = 9
  ├─ sum += BIT[4] = 9 + 10 = 19
  ├─ Remove LSB: 4 & -4 = 0100 & 1100 = 0100 = 4
  ├─ index = 4 - 4 = 0
  └─ Current sum = 19 (added arr[0..3])

Step 3: index = 0
  └─ Loop terminates (index ≤ 0)

=== QUERY RESULT ===
prefixSum(5) = 19 ✓

Verification:
  BIT[6] covers: arr[4] + arr[5] = 5 + 4 = 9
  BIT[4] covers: arr[0..3] = 3+2+(-1)+6 = 10
  Total: 9 + 10 = 19 ✓

Path: 6 → 4 → 0
Steps: 2 (O(log n))
```

***

### Point Update with Propagation

**Update:** `update(index=2, delta=5)` - Add 5 to arr[1]

```text
=== POINT UPDATE LOG: update(2, 5) ===

Current: arr[2] = -1
New: arr[2] = -1 + 5 = 4
Delta: +5

Convert to 1-indexed: index = 2 + 1 = 3

Step 1: index = 3 (binary: 0011)
  ├─ BIT[3] = -1
  ├─ BIT[3] += 5 = -1 + 5 = 4
  ├─ Add LSB: 3 & -3 = 0011 & 1101 = 0001 = 1
  ├─ index = 3 + 1 = 4
  └─ ✓ BIT[3]: -1 → 4

Step 2: index = 4 (binary: 0100)
  ├─ BIT[4] = 10
  ├─ BIT[4] += 5 = 10 + 5 = 15
  ├─ Add LSB: 4 & -4 = 0100 & 1100 = 0100 = 4
  ├─ index = 4 + 4 = 8
  └─ ✓ BIT[4]: 10 → 15

Step 3: index = 8 (binary: 1000)
  ├─ BIT[8] = 19
  ├─ BIT[8] += 5 = 19 + 5 = 24
  ├─ Add LSB: 8 & -8 = 1000 & 1000 = 1000 = 8
  ├─ index = 8 + 8 = 16 > n
  └─ ✓ BIT[8]: 19 → 24

=== UPDATE SUMMARY ===
Nodes Modified: 3 (indices 3, 4, 8)
Propagation Path: 3 → 4 → 8
All affected ranges updated correctly!

Before: [0, 3, 5, -1, 10, 5, 9, -3, 19]
After:  [0, 3, 5,  4, 15, 5, 9, -3, 24]
```

***

## 3. Union-Find Interactive Logging

### Union Operations with Path Compression

**Operations:** Build DSU with union operations

```text
=== UNION-FIND BUILD LOG ===

Initialize: n = 8 nodes (0-7)

Initial State:
  parent: [0, 1, 2, 3, 4, 5, 6, 7]
  rank:   [0, 0, 0, 0, 0, 0, 0, 0]
  
Each node is its own parent (8 separate components)

=== Operation 1: union(0, 1) ===

Step 1: find(0)
  ├─ parent[0] = 0
  └─ Root of 0 is 0

Step 2: find(1)
  ├─ parent[1] = 1
  └─ Root of 1 is 1

Step 3: Union by rank
  ├─ rootX = 0, rootY = 1
  ├─ rank[0] = 0, rank[1] = 0 (equal)
  ├─ Attach 1 under 0 (arbitrary choice)
  ├─ parent[1] = 0
  └─ rank[0] = 1 (increment rank of new root)

After union(0, 1):
  parent: [0, 0, 2, 3, 4, 5, 6, 7]
  rank:   [1, 0, 0, 0, 0, 0, 0, 0]
  
Components: 7 (merged 0↔1)
Tree: 0
      └─1

=== Operation 2: union(2, 3) ===

find(2) → root = 2
find(3) → root = 3
rank[2] = 0, rank[3] = 0
parent[3] = 2
rank[2] = 1

After union(2, 3):
  parent: [0, 0, 2, 2, 4, 5, 6, 7]
  rank:   [1, 0, 1, 0, 0, 0, 0, 0]
  
Components: 6
Trees: 0        2
       └─1      └─3

=== Operation 3: union(0, 2) ===

Step 1: find(0)
  └─ Root of 0 is 0

Step 2: find(2)
  └─ Root of 2 is 2

Step 3: Union by rank
  ├─ rank[0] = 1, rank[2] = 1 (equal)
  ├─ Attach 2 under 0
  ├─ parent[2] = 0
  └─ rank[0] = 2

After union(0, 2):
  parent: [0, 0, 0, 2, 4, 5, 6, 7]
  rank:   [2, 0, 1, 0, 0, 0, 0, 0]
  
Components: 5
Tree:     0
        /   \
       1     2
             └─3

=== Operation 4: union(4, 5) ===

find(4) → root = 4
find(5) → root = 5
parent[5] = 4
rank[4] = 1

After union(4, 5):
  parent: [0, 0, 0, 2, 4, 4, 6, 7]
  rank:   [2, 0, 1, 0, 1, 0, 0, 0]

Components: 4
Trees: 0           4
     /   \         └─5
    1     2
          └─3

=== Operation 5: union(3, 5) (with path compression) ===

Step 1: find(3)
  ├─ parent[3] = 2
  ├─ parent[2] = 0
  ├─ parent[0] = 0 ← Root found!
  ├─ Path compression: parent[3] = 0
  ├─ Path compression: parent[2] = 0
  └─ Root of 3 is 0

Step 2: find(5)
  ├─ parent[5] = 4
  ├─ parent[4] = 4 ← Root found!
  ├─ Path compression: parent[5] = 4
  └─ Root of 5 is 4

Step 3: Union by rank
  ├─ rootX = 0, rootY = 4
  ├─ rank[0] = 2, rank[4] = 1
  ├─ rank[0] > rank[4] → Attach 4 under 0
  └─ parent[4] = 0

After union(3, 5) with path compression:
  parent: [0, 0, 0, 0, 0, 4, 6, 7]
  rank:   [2, 0, 1, 0, 1, 0, 0, 0]

Components: 3
Tree:         0
         /  /  \  \
        1  2   3   4
                   └─5

Path Compression Effect:
  Before: 3→2→0 (length 2)
  After:  3→0   (length 1) ✓
```

***

## 4. Bloom Filter Interactive Logging

### Add and Query Operations

```text
=== BLOOM FILTER LOG ===

Configuration:
  Size (m): 20 bits
  Hash Functions (k): 3
  False Positive Rate: ~5%

Initial State:
  bits: [0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]

=== ADD: "apple" ===

Hash 1: hash("apple", 0) = 47 % 20 = 7
Hash 2: hash("apple", 1) = 33 % 20 = 13
Hash 3: hash("apple", 2) = 28 % 20 = 8

Set bits at positions: [7, 13, 8]

After adding "apple":
  bits: [0,0,0,0,0,0,0,1,1,0,0,0,0,1,0,0,0,0,0,0]
        Index:           7 8       13

=== ADD: "banana" ===

Hash 1: hash("banana", 0) = 52 % 20 = 12
Hash 2: hash("banana", 1) = 67 % 20 = 7
Hash 3: hash("banana", 2) = 41 % 20 = 1

Set bits at positions: [12, 7, 1]

After adding "banana":
  bits: [0,1,0,0,0,0,0,1,1,0,0,0,1,1,0,0,0,0,0,0]
        Index:   1       7 8    12 13

=== QUERY: "apple" ===

Hash 1: position 7  → bits[7] = 1 ✓
Hash 2: position 13 → bits[13] = 1 ✓
Hash 3: position 8  → bits[8] = 1 ✓

Result: TRUE (probably in set)
Status: ✓ Correct (apple was added)

=== QUERY: "orange" ===

Hash 1: hash("orange", 0) = 89 % 20 = 9
Hash 2: hash("orange", 1) = 44 % 20 = 4
Hash 3: hash("orange", 2) = 71 % 20 = 11

Check bits at positions: [9, 4, 11]
  bits[9] = 0 ✗

Result: FALSE (definitely not in set)
Status: ✓ Correct (orange was never added)

=== QUERY: "cherry" (False Positive Demo) ===

Hash 1: hash("cherry", 0) = 67 % 20 = 7
Hash 2: hash("cherry", 1) = 93 % 20 = 13
Hash 3: hash("cherry", 2) = 48 % 20 = 8

Check bits at positions: [7, 13, 8]
  bits[7] = 1 ✓ (from "apple" and "banana")
  bits[13] = 1 ✓ (from "apple")
  bits[8] = 1 ✓ (from "apple")

Result: TRUE (probably in set)
Status: ✗ FALSE POSITIVE! (cherry was never added)

Explanation: All 3 hash positions were set by other elements
Probability: ~5% based on configuration
```

***

## 5. LRU Cache Interactive Logging

### Complete Operation Trace

```text
=== LRU CACHE LOG (capacity=3) ===

Initial State:
  Cache: {}
  List: head ↔ tail
  Size: 0/3

=== PUT(1, 10) ===

Step 1: key=1 not in cache
Step 2: Cache not full (0 < 3)
Step 3: Create new node(1, 10)
Step 4: Add to head (MRU position)

After PUT(1, 10):
  Cache: {1→node1}
  List: head ↔ [1:10] ↔ tail
  Size: 1/3
  MRU: 1, LRU: 1

=== PUT(2, 20) ===

After PUT(2, 20):
  Cache: {1→node1, 2→node2}
  List: head ↔ [2:20] ↔ [1:10] ↔ tail
  Size: 2/3
  MRU: 2, LRU: 1

=== GET(1) ===

Step 1: key=1 found in cache
Step 2: Move node1 to head (recently accessed)

After GET(1):
  Cache: {1→node1, 2→node2}
  List: head ↔ [1:10] ↔ [2:20] ↔ tail
  Size: 2/3
  MRU: 1, LRU: 2
  Return: 10

=== PUT(3, 30) ===

After PUT(3, 30):
  Cache: {1→node1, 2→node2, 3→node3}
  List: head ↔ [3:30] ↔ [1:10] ↔ [2:20] ↔ tail
  Size: 3/3 (FULL)
  MRU: 3, LRU: 2

=== PUT(4, 40) - EVICTION ===

Step 1: key=4 not in cache
Step 2: Cache FULL (3/3)
Step 3: Evict LRU (key=2 at tail.prev)
  ├─ Remove node2 from list
  ├─ Delete cache[2]
  └─ Size = 2/3

Step 4: Add new node(4, 40) to head

After PUT(4, 40):
  Cache: {1→node1, 3→node3, 4→node4}
  List: head ↔ [4:40] ↔ [3:30] ↔ [1:10] ↔ tail
  Size: 3/3
  MRU: 4, LRU: 1
  Evicted: key=2 ✗

=== GET(2) ===

Step 1: key=2 not in cache
Return: -1 (not found)

Cache Miss! (key was evicted)

=== FINAL STATE ===
  Cache Contents: {1:10, 3:30, 4:40}
  Access Order: 4 (newest) → 3 → 1 (oldest)
  Next Eviction: key=1
```

***

## Debugging Best Practices

### Common Issues and Logging Solutions

**1. Segment Tree Index Errors:**
```text
ERROR: Query returns wrong result

DEBUG LOG:
  ├─ Check 0-indexed vs 1-indexed confusion
  ├─ Verify mid calculation: (L+R)/2
  ├─ Log each node visit with [L, R] bounds
  └─ Ensure proper overlap detection
```

**2. Fenwick Tree LSB Calculation:**
```text
ERROR: Wrong prefix sum

DEBUG LOG:
  ├─ Print binary representation of indices
  ├─ Log (i & -i) at each step
  ├─ Verify path: should traverse O(log n) nodes
  └─ Check 0-indexed vs 1-indexed conversion
```

**3. Union-Find Not Compressing:**
```text
ERROR: Slow find operations

DEBUG LOG:
  ├─ Count find() depth
  ├─ Verify path compression updates parent[]
  ├─ Check if rank/size optimization applied
  └─ Print tree structure after operations
```

**These interactive logs help visualize internal operations, making debugging and learning significantly easier. Add similar logging to your implementations for better understanding and faster bug detection!**[4][5][1]

[1](https://cp-algorithms.com/data_structures/segment_tree.html)
[2](https://www.geeksforgeeks.org/dsa/segment-tree-efficient-implementation/)
[3](https://codeforces.com/blog/entry/18051)
[4](https://visualgo.net/en/segmenttree)
[5](https://algo.monster/problems/segment_tree_intro)
[6](https://leetcode.com/discuss/interview-question/787180/segment-tree-implementation-python-range-query-min-max-sum-and-update-segment-tree)
[7](https://usaco.guide/adv/segtree-beats)
[8](https://www.geeksforgeeks.org/dsa/segment-tree-data-structure/)