🟢 Heapify: The Core Operation Maintaining Heap Properties
1. What is Heapify?
Heapify is the operation that restores the heap property after an insertion, deletion, or structural modification. It involves moving a single element up or down the tree until the heap property is satisfied.

Two Primary Operations:

Heapify-Up (Bubble Up): Move element up the tree.

Heapify-Down (Bubble Down): Move element down the tree.

2. Heapify-Up (Sift Up / Percolate Up)
Purpose
Restore heap property after inserting an element at the end of the array.

Algorithm (Min-Heap)
text
FUNCTION heapifyUp(idx)
    WHILE idx > 0 DO
        parentIdx = (idx - 1) / 2
        IF arr[idx] < arr[parentIdx] THEN
            SWAP(arr[idx], arr[parentIdx])
            idx = parentIdx
        ELSE
            BREAK
        END IF
    END WHILE
END FUNCTION
Execution Flow
text
Insert 1 into [5, 10, 8, 15, 20]

Step 1: Append → [5, 10, 8, 15, 20, 1]
        idx = 5

Step 2: Parent idx = (5-1)/2 = 2
        Compare 1 (child) < 8 (parent) → SWAP
        Array: [5, 10, 1, 15, 20, 8]
        idx = 2

Step 3: Parent idx = (2-1)/2 = 0
        Compare 1 (child) < 5 (parent) → SWAP
        Array: [1, 10, 5, 15, 20, 8]
        idx = 0

Step 4: idx == 0 → STOP
Complexity
Time: O(log n) worst-case (travel to root).

Space: O(1).

Key Points
Single comparison per level.

Maximum path length: height of tree = O(log n).

Early termination when heap property satisfied.

3. Heapify-Down (Sift Down / Percolate Down)
Purpose
Restore heap property after removing root or modifying an internal node.

Algorithm (Min-Heap)
text
FUNCTION heapifyDown(idx)
    WHILE 2 * idx + 1 < heapSize DO
        minIdx = idx
        leftChildIdx = 2 * idx + 1
        rightChildIdx = 2 * idx + 2
        
        // Find minimum among node and children
        IF arr[leftChildIdx] < arr[minIdx] THEN
            minIdx = leftChildIdx
        END IF
        
        IF rightChildIdx < heapSize AND arr[rightChildIdx] < arr[minIdx] THEN
            minIdx = rightChildIdx
        END IF
        
        // If smallest is not current node, swap and continue
        IF minIdx != idx THEN
            SWAP(arr[idx], arr[minIdx])
            idx = minIdx
        ELSE
            BREAK
        END IF
    END WHILE
END FUNCTION
Execution Flow
text
Remove root from [1, 3, 5, 7, 9, 10]

Step 1: Move last to root → [10, 3, 5, 7, 9]
        Remove last element
        idx = 0

Step 2: Children: idx 1 = 3, idx 2 = 5
        Min(10, 3, 5) = 3 at idx 1
        SWAP → [3, 10, 5, 7, 9]
        idx = 1

Step 3: Children: idx 3 = 7, idx 4 = 9
        Min(10, 7, 9) = 7 at idx 3
        SWAP → [3, 7, 5, 10, 9]
        idx = 3

Step 4: No left child (2*3+1 = 7 >= size 5) → STOP
Complexity
Time: O(log n) worst-case (travel to leaf).

Space: O(1).

Key Points
Compare with both children; choose smaller (min-heap).

Continue until leaf or heap property restored.

Two comparisons per level; higher constant than heapify-up.

4. Build Heap (Heapify All)
Purpose
Convert arbitrary array into valid heap in O(n) time (faster than n insertions).

Algorithm
text
FUNCTION buildHeap(arr)
    heapSize = arr.length
    
    // Start from last non-leaf node
    FOR i = (heapSize / 2) - 1 DOWN TO 0 DO
        heapifyDown(i)
    END FOR
    
    RETURN arr
END FUNCTION
Why Start from Last Non-Leaf?
text
Array: [7, 3, 5, 1, 9, 2, 8]
Index:  0  1  2  3  4  5  6

Leaves: indices 3, 4, 5, 6 (no children to heapify)
Last non-leaf: index (7/2)-1 = 2

Tree structure:
        7(0)
       /    \
     3(1)   5(2)
    /  \    /  \
  1(3) 9(4) 2(5) 8(6)
Start heapify at index 2 (value 5), then 1, then 0.

Execution
text
Initial: [7, 3, 5, 1, 9, 2, 8]

i=2 (node 5): Children 2, 8. Min(5,2,8)=2. SWAP.
Result: [7, 3, 2, 1, 9, 5, 8]

i=1 (node 3): Children 1, 9. Min(3,1,9)=1. SWAP.
Result: [7, 1, 2, 3, 9, 5, 8]
Then heapify from idx 3: no children. Stop.

i=0 (node 7): Children 1, 2. Min(7,1,2)=1. SWAP.
Result: [1, 7, 2, 3, 9, 5, 8]
Then heapify from idx 1: Children 3, 9. Min(7,3,9)=3. SWAP.
Result: [1, 3, 2, 7, 9, 5, 8]
Then heapify from idx 3: no children. Stop.

Final Min-Heap: [1, 3, 2, 7, 9, 5, 8]
Complexity
Time: O(n) (not O(n log n) as naive insertion would be).

Space: O(1) (in-place).

Why O(n) not O(n log n)?
Most nodes are at lower levels; heapify-down cost is cheap for them.

text
For complete binary tree with n nodes:
- ~n/2 nodes at depth h-1: cost O(1) heapify
- ~n/4 nodes at depth h-2: cost O(1) heapify
- ~n/8 nodes at depth h-3: cost O(2) heapify
- ...

Total: O(n/2 * 1 + n/4 * 1 + n/8 * 2 + ...) = O(n)
5. Heapify Variations
a. Lazy Heapify
Defer heapification until absolutely needed.

Useful for insertions before extraction.

b. Incremental Heapify
Gradually restore heap property in background.

Used in concurrent/parallel systems.

c. Partial Heapify
Only heapify affected regions (e.g., after decrease-key).

Optimization for specialized heaps.

6. Heapify in Different Heap Types
Binary Heap
Standard heapify-up/down.

Most common.

D-ary Heap
Children at indices di+1 to di+d.

Heapify-down compares with all d children; choose minimum.

Fibonacci Heap
Cascading cuts; lazy consolidation.

Complex heapify; rarely used in practice.

Binomial Heap
Heapify within binomial trees; merge trees.

Efficient merge; heapify integrated into merge.

7. Common Pitfalls in Heapify
Mistake 1: Heapify-down compares only with left child.

text
// WRONG
IF arr[leftChildIdx] < arr[idx] THEN
    SWAP(...)
    idx = leftChildIdx
END IF

// CORRECT: Check both children; pick minimum
IF arr[leftChildIdx] < arr[minIdx] THEN minIdx = leftChildIdx
IF arr[rightChildIdx] < arr[minIdx] THEN minIdx = rightChildIdx
IF minIdx != idx THEN SWAP(...)
Mistake 2: Off-by-one errors in indices.

text
// WRONG: Last non-leaf might not be heapified
FOR i = (heapSize / 2) DOWN TO 0 DO  // Missing -1

// CORRECT
FOR i = (heapSize / 2) - 1 DOWN TO 0 DO
Mistake 3: Not checking boundary conditions.

text
// WRONG: May access out-of-bounds
rightChildIdx = 2 * idx + 2
IF arr[rightChildIdx] < arr[minIdx] THEN ...  // No bounds check

// CORRECT
IF rightChildIdx < heapSize AND arr[rightChildIdx] < arr[minIdx] THEN ...
Mistake 4: Infinite loop in heapify-up.

text
// WRONG: No stopping condition
WHILE idx > 0 DO
    parentIdx = (idx - 1) / 2
    IF arr[idx] < arr[parentIdx] THEN
        SWAP(...)
        idx = parentIdx
    // Missing ELSE BREAK
END WHILE

// CORRECT
WHILE idx > 0 DO
    parentIdx = (idx - 1) / 2
    IF arr[idx] < arr[parentIdx] THEN
        SWAP(...)
        idx = parentIdx
    ELSE
        BREAK
    END IF
END WHILE
8. Interview Patterns
Common Questions:

Implement Heapify: Push/pop with heapify-up/down.

Build Heap: Optimize from O(n log n) naive to O(n).

Complexity Analysis: Why O(log n) vs O(n)?

Edge Cases: Single element, empty heap, duplicates.

D-ary Heaps: Modify heapify for d children.

FAANG Expectations:

Implement both heapify-up and heapify-down from scratch.

Explain O(n) vs O(n log n) for build heap.

Handle boundary conditions carefully.

Discuss trade-offs: insertion (heapify-up easy) vs deletion (heapify-down complex).

9. Best Practices
Boundary checks: Always validate child indices before access.

Early termination: Stop heapify once heap property restored.

Consistent comparisons: Use same comparator throughout.

Test thoroughly: Edge cases, single element, power-of-2 sizes.

Prefer array-backed: Cache-friendly, no pointer overhead.

10. Summary
Heapify-Up: O(log n), used in push; moves up tree comparing with parent.

Heapify-Down: O(log n), used in pop; moves down tree comparing with children.

Build Heap: O(n), converts array to heap by heapifying from last non-leaf downward.

Mastery: Implement both operations flawlessly; understand why build heap is O(n); handle all edge cases.