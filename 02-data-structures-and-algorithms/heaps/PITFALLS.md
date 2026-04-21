🟢 Heap Pitfalls: Common Bugs, Edge Cases & Defensive Coding
1. Off-by-One Errors in Index Calculations
Description:
Incorrect parent/child indices; accessing wrong elements or out-of-bounds.

Impact:
Wrong heap structure, lost elements, crashes.

Prevention:
Double-check formulas: parent = (i-1)/2, left = 2i+1, right = 2i+2.

text
// Bug: Incorrect parent calculation
parentIdx = (i - 1) / 3  // WRONG (should be /2)

// Bug: Off-by-one in loop
FOR i = heapSize / 2 DOWN TO 0 DO  // WRONG; may skip last non-leaf
FOR i = (heapSize / 2) - 1 DOWN TO 0 DO  // CORRECT

// Bug: Forgetting boundary check
IF rightChildIdx < heapSize THEN
    // Compare with right child
2. Forgetting to Check Boundaries
Description:
Accessing arr[idx] without verifying idx < heapSize.

Impact:
Array out-of-bounds exception, undefined behavior.

Prevention:
Always validate indices before dereferencing.

text
// Bug: No bounds check
FUNCTION heapifyDown(idx)
    WHILE 2 * idx + 1 < heapSize DO
        leftChildIdx = 2 * idx + 1
        rightChildIdx = 2 * idx + 2
        // BUG: Access right child without checking bounds
        IF arr[rightChildIdx] < arr[minIdx] THEN  // Crash if rightChildIdx >= heapSize

// Fixed
        IF rightChildIdx < heapSize AND arr[rightChildIdx] < arr[minIdx] THEN
            minIdx = rightChildIdx
        END IF
3. Heap Property Violation After Modification
Description:
Not restoring heap property after insert/delete/modify; violating min/max-heap invariant.

Impact:
Wrong peek/pop results, algorithm failures.

Prevention:
Always heapify-up after insertion, heapify-down after deletion.

text
// Bug: Modify element but forget to heapify
FUNCTION decreaseValue(idx, newVal)
    arr[idx] = newVal  // Only this; no heapify-up
    // WRONG: Heap property may be violated

// Fixed
FUNCTION decreaseValue(idx, newVal)
    arr[idx] = newVal
    heapifyUp(idx)  // Restore heap property
END FUNCTION
4. Edge Cases: Empty, Single, Two-Element Heaps
Description:
Not testing empty heap, single element, two elements.

Impact:
Crashes, index errors on edge cases.

Prevention:
Always test: empty, single, two elements, power-of-2 sizes.

text
// Bug: Assume heap always has elements
FUNCTION peek()
    RETURN arr[0]  // Crashes if empty

// Fixed
FUNCTION peek()
    IF heapSize == 0 THEN
        ERROR "Heap empty"
    END IF
    RETURN arr[0]
END FUNCTION
5. Comparing Only Left Child in Heapify-Down
Description:
Heapify-down compares only left child; misses right child (may be smaller).

Impact:
Heap property violated; wrong structure.

Prevention:
Always check both children; choose minimum/maximum.

text
// Bug: Only compare left child
FUNCTION heapifyDown(idx)
    leftChildIdx = 2 * idx + 1
    IF leftChildIdx < heapSize AND arr[leftChildIdx] < arr[idx] THEN
        SWAP(arr[idx], arr[leftChildIdx])
        heapifyDown(leftChildIdx)
    END IF
    // WRONG: Ignores right child

// Fixed
FUNCTION heapifyDown(idx)
    minIdx = idx
    leftChildIdx = 2 * idx + 1
    rightChildIdx = 2 * idx + 2
    
    IF leftChildIdx < heapSize AND arr[leftChildIdx] < arr[minIdx] THEN
        minIdx = leftChildIdx
    END IF
    IF rightChildIdx < heapSize AND arr[rightChildIdx] < arr[minIdx] THEN
        minIdx = rightChildIdx
    END IF
    
    IF minIdx != idx THEN
        SWAP(arr[idx], arr[minIdx])
        heapifyDown(minIdx)
    END IF
END FUNCTION
6. Missing Early Termination in Heapify
Description:
Not stopping heapify-up/down when condition met; unnecessary iterations.

Impact:
Performance degradation; worse than O(log n).

Prevention:
Add BREAK or condition to exit loop.

text
// Bug: Missing BREAK
FUNCTION heapifyUp(idx)
    WHILE idx > 0 DO
        parentIdx = (idx - 1) / 2
        IF arr[idx] < arr[parentIdx] THEN
            SWAP(arr[idx], arr[parentIdx])
            idx = parentIdx
        // Missing ELSE BREAK

// Fixed
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
7. Size/Count Not Updated
Description:
Forgetting to update heapSize after push/pop.

Impact:
Wrong array bounds, lost elements.

Prevention:
Always update heapSize in push/pop.

text
// Bug: heapSize not decremented
FUNCTION pop()
    minVal = arr[0]
    arr[0] = arr[heapSize - 1]
    // Missing: heapSize -= 1
    heapifyDown(0)
    RETURN minVal

// Fixed
FUNCTION pop()
    minVal = arr[0]
    arr[0] = arr[heapSize - 1]
    heapSize -= 1
    heapifyDown(0)
    RETURN minVal
END FUNCTION
8. Max-Heap Implementation Error
Description:
Max-heap logic wrong; comparing left < right instead of left > right.

Impact:
Min-heap created instead of max-heap.

Prevention:
Careful comparison operators; test thoroughly.

text
// Bug: Wrong comparisons for max-heap
FUNCTION heapifyDownMaxHeap(idx)
    maxIdx = idx
    leftChildIdx = 2 * idx + 1
    rightChildIdx = 2 * idx + 2
    
    IF leftChildIdx < heapSize AND arr[leftChildIdx] < arr[maxIdx] THEN  // WRONG (<)
        maxIdx = leftChildIdx
    END IF

// Fixed
FUNCTION heapifyDownMaxHeap(idx)
    maxIdx = idx
    leftChildIdx = 2 * idx + 1
    rightChildIdx = 2 * idx + 2
    
    IF leftChildIdx < heapSize AND arr[leftChildIdx] > arr[maxIdx] THEN  // CORRECT (>)
        maxIdx = leftChildIdx
    END IF
    // ... similar for right child
END FUNCTION
9. Build Heap Starting from Wrong Index
Description:
Starting heapify from index 0 instead of last non-leaf; inefficient or buggy.

Impact:
O(n log n) instead of O(n); unnecessary work.

Prevention:
Start from (heapSize / 2) - 1.

text
// Bug: Start from 0
FUNCTION buildHeap(arr)
    FOR i = 0 TO arr.length - 1 DO
        heapifyDown(i)
    END FOR

// Fixed: Start from last non-leaf
FUNCTION buildHeap(arr)
    FOR i = (arr.length / 2) - 1 DOWN TO 0 DO
        heapifyDown(i)
    END FOR
END FUNCTION
10. Not Handling Heap Overflow
Description:
Pushing to heap that's already at capacity; no resize or error handling.

Impact:
Lost data, silent corruption.

Prevention:
Check capacity; resize or error.

text
// Bug: No overflow check
FUNCTION push(val)
    arr[heapSize] = val  // Crash if heapSize >= arr.capacity

// Fixed
FUNCTION push(val)
    IF heapSize == arr.capacity THEN
        RESIZE(arr, arr.capacity * 2)
    END IF
    arr[heapSize] = val
    heapSize += 1
    heapifyUp(heapSize - 1)
END FUNCTION
11. Heap Underflow (Pop from Empty)
Description:
Popping from empty heap without checking.

Impact:
Undefined behavior, wrong values returned.

Prevention:
Check heapSize > 0 before pop.

text
// Bug: No underflow check
FUNCTION pop()
    RETURN arr[0]  // Crash if heapSize == 0

// Fixed
FUNCTION pop()
    IF heapSize == 0 THEN
        ERROR "Heap underflow"
    END IF
    RETURN arr[0]
END FUNCTION
12. Inconsistent Min/Max-Heap Throughout Code
Description:
Mixing min-heap and max-heap logic; one function uses <, another >.

Impact:
Unpredictable behavior, wrong results.

Prevention:
Choose one; consistently apply comparisons everywhere.

text
// Bug: Inconsistent
FUNCTION pushMinHeap(val) { ... compare with < ... }
FUNCTION popMaxHeap() { ... compare with > ... }
// Applied to same heap → broken

// Fixed: Consistent
// Use only min-heap or only max-heap for a given heap object
CLASS MinHeap { ... uses < }
CLASS MaxHeap { ... uses > }
13. Not Validating Heap Property
Description:
After operations, not verifying heap property still holds.

Impact:
Silent data corruption; bugs hard to trace.

Prevention:
Add validation function for testing.

text
FUNCTION validateHeap()
    FOR i = 0 TO (heapSize / 2) - 1 DO
        leftChildIdx = 2 * i + 1
        rightChildIdx = 2 * i + 2
        
        IF leftChildIdx < heapSize AND arr[i] > arr[leftChildIdx] THEN
            RETURN false  // Min-heap violated
        END IF
        IF rightChildIdx < heapSize AND arr[i] > arr[rightChildIdx] THEN
            RETURN false
        END IF
    END FOR
    RETURN true
END FUNCTION
14. Infinite Loop in Heapify
Description:
Logic error causes heapify-up/down to never terminate.

Impact:
Program hangs indefinitely.

Prevention:
Ensure idx changes each iteration; use clear termination condition.

text
// Bug: idx doesn't change
FUNCTION heapifyUp(idx)
    WHILE idx > 0 AND arr[idx] < arr[parent(idx)] DO
        SWAP(arr[idx], arr[parent(idx)])
        // Missing: idx = parent(idx)
        // Infinite loop

// Fixed
FUNCTION heapifyUp(idx)
    WHILE idx > 0 AND arr[idx] < arr[parent(idx)] DO
        SWAP(arr[idx], arr[parent(idx)])
        idx = parent(idx)  // Update idx
    END WHILE
END FUNCTION
15. Duplicate Values Handling
Description:
Not handling duplicates; duplicates break heap invariant (if assumed unique).

Impact:
Multiple returns of same value; wrong counts.

Prevention:
Allow duplicates; or use secondary key to break ties.

16. Type Mismatch / Casting Errors
Description:
Integer division (5 / 2 = 2) vs float division; casting issues.

Impact:
Wrong indices, parent/child relationships broken.

Prevention:
Use integer division explicitly; check types.

text
// Bug: Float division
parentIdx = (idx - 1) / 2.0  // Results in float; cast to int?

// Fixed: Integer division
parentIdx = (idx - 1) / 2  // Implicit integer division
17. Not Clearing Heap Data Structure
Description:
After heap operations, stale data remains; memory leak or incorrect state.

Impact:
Incorrect results if reusing heap; memory leak.

Prevention:
Implement clear() or reset().

text
FUNCTION clear()
    FOR i = 0 TO heapSize - 1 DO
        arr[i] = null
    END FOR
    heapSize = 0
END FUNCTION
18. Performance Pitfalls
Choosing binary heap for merge-heavy workload: Use binomial heap.

Repeated pop on large heap: Consider lazy deletion.

No early termination in search: Stop when found, not after full heapify.

19. Interview-Specific Pitfalls
Misunderstanding problem: "Top-K" doesn't always mean max-heap; context matters.

Forgetting edge cases: Empty, single element, duplicates.

Inefficient heap choice: Binary heap sufficient for 95% of interviews.

Not explaining complexity: O(n) build vs O(n log n) insertion.

20. Best Practices
Always check boundaries before array access.

Test edge cases: empty, single, two elements.

Use consistent heap type (min or max) throughout.

Validate heap property in tests.

Pre-allocate capacity when size known.

Use language's built-in heaps when available (heapq, PriorityQueue).

Master these pitfalls to write bulletproof heap code, acing interviews and avoiding subtle bugs that waste hours of debugging!

