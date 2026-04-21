🟢 Interactive Logging: Heaps – Step-by-Step Operation Traces
1. Min-Heap Push Operations
text
[Initialize] Min-Heap: []

[Push 5]
  [Append] 5 to end → [5]
  [HeapifyUp] idx=0, parent=none → no swap
  [State] [5]

[Push 3]
  [Append] 3 to end → [5, 3]
  [HeapifyUp] idx=1, parent idx=(1-1)/2=0
    [Compare] 3 < 5 → SWAP
  [After Swap] [3, 5]
  [HeapifyUp] idx=0, parent=none → stop
  [State] [3, 5]

[Push 7]
  [Append] 7 to end → [3, 5, 7]
  [HeapifyUp] idx=2, parent idx=(2-1)/2=0
    [Compare] 7 < 3? No → stop
  [State] [3, 5, 7]

[Push 1]
  [Append] 1 to end → [3, 5, 7, 1]
  [HeapifyUp] idx=3, parent idx=(3-1)/2=1
    [Compare] 1 < 5 → SWAP
  [After Swap] [3, 1, 7, 5]
  [HeapifyUp] idx=1, parent idx=(1-1)/2=0
    [Compare] 1 < 3 → SWAP
  [After Swap] [1, 3, 7, 5]
  [HeapifyUp] idx=0, parent=none → stop
  [State] [1, 3, 7, 5]

[Final Heap Structure]
        1
       / \
      3   7
     /
    5
2. Min-Heap Pop (Delete Min) Operations
text
[Heap] [1, 3, 7, 5]

[Pop]
  [Extract] min = 1
  [Move Last to Root] move 5 to position 0 → [5, 3, 7]
  [HeapifyDown] idx=0
    [Left Child] idx=1, value=3
    [Right Child] idx=2, value=7
    [Min of (5, 3, 7)] = 3 at idx=1
    [5 > 3] → SWAP
  [After Swap] [3, 5, 7]
  [HeapifyDown] idx=1
    [Left Child] idx=3 (out of bounds)
    [Stop]
  [Returned] 1
  [Final Heap] [3, 5, 7]

[State After Pop]
      3
     / \
    5   7
3. Build Heap from Array
text
[Array] [7, 3, 5, 1, 9, 2, 8]
[n] = 7
[Start from last non-leaf] i = (7/2)-1 = 2

[Iteration 1: i=2]
  [Node] arr[2]=5, children: idx 5=2, idx 6=8
  [Min] min(5, 2, 8) = 2 at idx 5
  [5 > 2] → SWAP
  [Array] [7, 3, 2, 1, 9, 5, 8]
  [HeapifyDown] continue from idx 5 → no more children

[Iteration 2: i=1]
  [Node] arr[1]=3, children: idx 3=1, idx 4=9
  [Min] min(3, 1, 9) = 1 at idx 3
  [3 > 1] → SWAP
  [Array] [7, 1, 2, 3, 9, 5, 8]
  [HeapifyDown] continue from idx 3 → no more children

[Iteration 3: i=0]
  [Node] arr[0]=7, children: idx 1=1, idx 2=2
  [Min] min(7, 1, 2) = 1 at idx 1
  [7 > 1] → SWAP
  [Array] [1, 7, 2, 3, 9, 5, 8]
  [HeapifyDown] continue from idx 1
    [Node] arr[1]=7, children: idx 3=3, idx 4=9
    [Min] min(7, 3, 9) = 3 at idx 3
    [7 > 3] → SWAP
  [Array] [1, 3, 2, 7, 9, 5, 8]
  [HeapifyDown] continue from idx 3
    [Node] arr[3]=7, children: idx 7, 8 (out of bounds)
    [Stop]

[Final Min-Heap] [1, 3, 2, 7, 9, 5, 8]

[Heap Structure]
        1
       / \
      3   2
     / \ / \
    7  9 5  8
4. Heap Sort Execution
text
[Array] [4, 2, 7, 1, 5, 3]

[Phase 1: Build Max-Heap]
  [Build from array]
  [After Heapify] [7, 5, 4, 1, 2, 3]

[Phase 2: Extract elements]

[Iteration 1]
  [Max at root] 7
  [SWAP] arr[0] ↔ arr[5] → [3, 5, 4, 1, 2, 7]
  [Heapify] from idx 0 in range [0, 4]
  [Result] [5, 3, 4, 1, 2, 7]

[Iteration 2]
  [Max at root] 5
  [SWAP] arr[0] ↔ arr[4] → [2, 3, 4, 1, 5, 7]
  [Heapify] from idx 0 in range [0, 3]
  [Result] [4, 3, 2, 1, 5, 7]

[Iteration 3]
  [Max at root] 4
  [SWAP] arr[0] ↔ arr[3] → [1, 3, 2, 4, 5, 7]
  [Heapify] from idx 0 in range [0, 2]
  [Result] [3, 1, 2, 4, 5, 7]

[Iteration 4]
  [Max at root] 3
  [SWAP] arr[0] ↔ arr[2] → [2, 1, 3, 4, 5, 7]
  [Heapify] from idx 0 in range [0, 1]
  [Result] [2, 1, 3, 4, 5, 7]

[Iteration 5]
  [Max at root] 2
  [SWAP] arr[0] ↔ arr[1] → [1, 2, 3, 4, 5, 7]
  [Heapify] from idx 0 in range [0, 0]
  [No change]

[Final Sorted Array] [1, 2, 3, 4, 5, 7]
5. Kth Largest Element (Min-Heap of Size K)
text
[Array] [3, 2, 1, 5, 6, 4, 7], k=3

[Build Min-Heap of size k=3 with first 3 elements]
  [Push 3] [3]
  [Push 2] [2, 3]
  [Push 1] [1, 3, 2]

[Heap State] [1, 3, 2]
  Min: 1, Max (peek top): 1

[Process Remaining Elements]

[i=3, element=5]
  [5 > 1 (min)?] Yes
  [Pop] 1, push 5 → [2, 3, 5]
  [Heap] [2, 3, 5]

[i=4, element=6]
  [6 > 2 (min)?] Yes
  [Pop] 2, push 6 → [3, 5, 6]
  [Heap] [3, 5, 6]

[i=5, element=4]
  [4 > 3 (min)?] Yes
  [Pop] 3, push 4 → [4, 5, 6]
  [Heap] [4, 5, 6]

[i=6, element=7]
  [7 > 4 (min)?] Yes
  [Pop] 4, push 7 → [5, 6, 7]
  [Heap] [5, 6, 7]

[Kth Largest] peek() = 5
[Top 3 largest in array] 5, 6, 7 ✓
6. Median of Data Stream (Two Heaps)
text
[Max-Heap Left: [], Min-Heap Right: []]

[Add 1]
  [Empty or 1 <= maxLeft.peek()?] Empty → push to left
  [Max-Heap Left] [1]
  [Min-Heap Right] []
  [Median] 1.0

[Add 3]
  [3 <= maxLeft.peek() (1)?] No → push to right
  [Max-Heap Left] [1]
  [Min-Heap Right] [3]
  [Balance] left=1, right=1 → balanced
  [Median] (1 + 3) / 2 = 2.0

[Add 2]
  [2 <= maxLeft.peek() (1)?] No → push to right
  [Min-Heap Right] [3, 2]
  [After push] [2, 3]
  [Balance Check] left=1, right=2 → right > left
    [Pop from right] 2, push to left
  [Max-Heap Left] [2, 1]
  [Min-Heap Right] [3]
  [Median] maxLeft.peek() = 2

[Add 4]
  [4 <= maxLeft.peek() (2)?] No → push to right
  [Min-Heap Right] [3, 4]
  [Balance] left=2, right=2 → balanced
  [Median] (2 + 3) / 2 = 2.5

[Add 5]
  [5 <= maxLeft.peek() (2)?] No → push to right
  [Min-Heap Right] [3, 4, 5]
  [After push] [3, 4, 5]
  [Balance] left=2, right=3 → right > left
    [Pop from right] 3, push to left
  [Max-Heap Left] [3, 2, 1] → [3, 2, 1]
  [Min-Heap Right] [4, 5]
  [Median] maxLeft.peek() = 3

[Final State]
  Max-Heap Left: [3, 2, 1] (stores 1, 2, 3)
  Min-Heap Right: [4, 5] (stores 4, 5)
  Median: 3.0
7. Top K Frequent Elements
text
[Array] [1, 1, 1, 2, 2, 3], k=2

[Frequency Map]
  1: 3
  2: 2
  3: 1

[Min-Heap by Frequency]

[Insert (3, 1)]
  [Heap] [(3, 1)]

[Insert (2, 2)]
  [Heap] [(2, 2), (3, 1)]
  [After HeapifyUp] [(2, 2), (3, 1)]

[Insert (1, 3)]
  [Heap] [(1, 3), (3, 1), (2, 2)]
  [After HeapifyUp] [(1, 3), (3, 1), (2, 2)]
  [Size > k] (3 > 2) → Pop min
  [Pop] (1, 3)
  [Heap] [(2, 2), (3, 1)]

[Result]
  [Remaining in heap] (2, 2), (3, 1)
  [Top K Frequent] [2, 1] (in any order)
8. Merge K Sorted Lists
text
[Lists]
  List 1: 1 → 4 → 5
  List 2: 1 → 3 → 4
  List 3: 2 → 6

[Min-Heap (value, node)]
  [Initial] [(1, L1[0]), (1, L2[0]), (2, L3[0])]

[Merge Process]

[Extract min] (1, L1[0])
  [Add to result] 1
  [Next from L1] 4
  [Push] (4, L1[1])
  [Heap] [(1, L2[0]), (2, L3[0]), (4, L1[1])]

[Extract min] (1, L2[0])
  [Add to result] 1 → 1
  [Next from L2] 3
  [Push] (3, L2[1])
  [Heap] [(2, L3[0]), (3, L2[1]), (4, L1[1])]

[Extract min] (2, L3[0])
  [Add to result] 1 → 1 → 2
  [Next from L3] 6
  [Push] (6, L3[1])
  [Heap] [(3, L2[1]), (4, L1[1]), (6, L3[1])]

[Extract min] (3, L2[1])
  [Add to result] 1 → 1 → 2 → 3
  [Next from L2] 4
  [Push] (4, L2[2])
  [Heap] [(4, L1[1]), (4, L2[2]), (6, L3[1])]

[Extract min] (4, L1[1])
  [Add to result] 1 → 1 → 2 → 3 → 4
  [Next from L1] 5
  [Push] (5, L1[2])
  [Heap] [(4, L2[2]), (5, L1[2]), (6, L3[1])]

[Extract min] (4, L2[2])
  [Add to result] 1 → 1 → 2 → 3 → 4 → 4
  [Next from L2] null (end of list)
  [Heap] [(5, L1[2]), (6, L3[1])]

[Extract min] (5, L1[2])
  [Add to result] 1 → 1 → 2 → 3 → 4 → 4 → 5
  [Next from L1] null
  [Heap] [(6, L3[1])]

[Extract min] (6, L3[1])
  [Add to result] 1 → 1 → 2 → 3 → 4 → 4 → 5 → 6
  [Next from L3] null
  [Heap] []

[Final Merged List] 1 → 1 → 2 → 3 → 4 → 4 → 5 → 6
9. Priority Queue (Task Scheduling)
text
[Priority Queue - Min-Heap by Priority]

[Enqueue Task A, priority=5]
  [Push] (5, A)
  [Heap] [(5, A)]

[Enqueue Task B, priority=2]
  [Push] (2, B)
  [HeapifyUp] 2 < 5 → Swap
  [Heap] [(2, B), (5, A)]

[Enqueue Task C, priority=8]
  [Push] (8, C)
  [HeapifyUp] 8 > 2, no swap
  [Heap] [(2, B), (5, A), (8, C)]

[Enqueue Task D, priority=1]
  [Push] (1, D)
  [HeapifyUp] 1 < 5 → Swap
  [After 1 swap] [(2, B), (1, D), (8, C), (5, A)]
  [HeapifyUp] 1 < 2 → Swap
  [After 2 swap] [(1, D), (2, B), (8, C), (5, A)]
  [Heap] [(1, D), (2, B), (8, C), (5, A)]

[Dequeue] 
  [Pop] (1, D)
  [Result] Task D with priority 1
  [Heap] [(2, B), (5, A), (8, C)]

[Dequeue]
  [Pop] (2, B)
  [Result] Task B with priority 2
  [Heap] [(5, A), (8, C)]

[Execution Order] D (pri 1) → B (pri 2) → A (pri 5) → C (pri 8)
10. Huffman Coding Build Trace
text
[Frequencies]
  a: 5
  b: 3
  c: 2
  d: 1

[Min-Heap - Initial]
  [(1, d), (2, c), (3, b), (5, a)]

[Build Huffman Tree]

[Iteration 1]
  [Pop two minimums] (1, d), (2, c)
  [Merge] new node (3, internal) with left=d, right=c
  [Push] (3, internal1)
  [Heap] [(3, internal1), (3, b), (5, a)]

[Iteration 2]
  [Pop two minimums] (3, internal1), (3, b)
  [Merge] new node (6, internal2)
  [Push] (6, internal2)
  [Heap] [(5, a), (6, internal2)]

[Iteration 3]
  [Pop two minimums] (5, a), (6, internal2)
  [Merge] new node (11, root)
  [Push] (11, root)
  [Heap] [(11, root)]

[Final Tree]
              11
            /    \
           a(5)  internal2(6)
                  /         \
            internal1(3)     b(3)
             /       \
            d(1)     c(2)

[Huffman Codes]
  a: 0 (1 bit)
  b: 11 (2 bits)
  c: 101 (3 bits)
  d: 100 (3 bits)
11. Dijkstra's Algorithm with Min-Heap
text
[Graph]
  1 -- 4 -- 2
  |    |   /
  2    1  1
  |         |
  3 --- 5 - 4

[Initialize]
  distances = {1: 0, 2: ∞, 3: ∞, 4: ∞}
  heap = [(0, 1)]

[Extract] (0, 1)
  [Process neighbors of 1]
    [2] cost = 0 + 4 = 4 < ∞ → Update, push (4, 2)
    [3] cost = 0 + 2 = 2 < ∞ → Update, push (2, 3)
  [Heap] [(2, 3), (4, 2)]

[Extract] (2, 3)
  [Process neighbors of 3]
    [4] cost = 2 + 5 = 7 < ∞ → Update, push (7, 4)
  [Heap] [(4, 2), (7, 4)]

[Extract] (4, 2)
  [Process neighbors of 2]
    [4] cost = 4 + 1 = 5 < 7 → Update, push (5, 4)
  [Heap] [(5, 4), (7, 4)]

[Extract] (5, 4)
  [4 already visited or best distance reached]
  [Mark visited]

[Extract] (7, 4)
  [4 already visited, skip]

[Final Shortest Distances]
  1: 0
  2: 4 (path: 1→2)
  3: 2 (path: 1→3)
  4: 5 (path: 1→2→4)
These interactive traces illuminate the internal mechanics of heap operations—see how elements move, state evolves, and algorithms execute step by step. Master these patterns for heap mastery!

