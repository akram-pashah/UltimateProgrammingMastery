🟢 Pseudocode: Heaps – Core Operations to Advanced Algorithms
1. Min-Heap Node and Structure
text
CLASS MinHeap
    arr = []
    
    FUNCTION parent(i)
        RETURN (i - 1) / 2
    END FUNCTION
    
    FUNCTION leftChild(i)
        RETURN 2 * i + 1
    END FUNCTION
    
    FUNCTION rightChild(i)
        RETURN 2 * i + 2
    END FUNCTION
    
    FUNCTION size()
        RETURN arr.length
    END FUNCTION
    
    FUNCTION isEmpty()
        RETURN arr.length == 0
    END FUNCTION
END CLASS
2. Push (Insert) Operation
text
FUNCTION push(val)
    arr.append(val)
    heapifyUp(arr.length - 1)
END FUNCTION

FUNCTION heapifyUp(idx)
    WHILE idx > 0 DO
        parentIdx = parent(idx)
        IF arr[idx] < arr[parentIdx] THEN
            SWAP(arr[idx], arr[parentIdx])
            idx = parentIdx
        ELSE
            BREAK
        END IF
    END WHILE
END FUNCTION
Complexity: O(log n).

3. Pop (Delete Min) Operation
text
FUNCTION pop()
    IF arr.length == 0 THEN
        ERROR "Heap underflow"
    END IF
    
    minVal = arr[0]
    arr[0] = arr[arr.length - 1]
    arr.removeLast()
    
    IF arr.length > 0 THEN
        heapifyDown(0)
    END IF
    
    RETURN minVal
END FUNCTION

FUNCTION heapifyDown(idx)
    WHILE 2 * idx + 1 < arr.length DO
        minIdx = idx
        leftChildIdx = 2 * idx + 1
        rightChildIdx = 2 * idx + 2
        
        IF arr[leftChildIdx] < arr[minIdx] THEN
            minIdx = leftChildIdx
        END IF
        
        IF rightChildIdx < arr.length AND arr[rightChildIdx] < arr[minIdx] THEN
            minIdx = rightChildIdx
        END IF
        
        IF minIdx != idx THEN
            SWAP(arr[idx], arr[minIdx])
            idx = minIdx
        ELSE
            BREAK
        END IF
    END WHILE
END FUNCTION
Complexity: O(log n).

4. Peek Operation
text
FUNCTION peek()
    IF arr.length == 0 THEN
        ERROR "Heap empty"
    END IF
    RETURN arr[0]
END FUNCTION
Complexity: O(1).

5. Build Heap from Array (Heapify)
text
FUNCTION buildHeap(inputArr)
    arr = COPY(inputArr)
    
    // Start from last non-leaf node and heapify down
    FOR i = (arr.length / 2) - 1 DOWN TO 0 DO
        heapifyDown(i)
    END FOR
    
    RETURN arr
END FUNCTION

FUNCTION heapifyDown(idx)
    n = arr.length
    WHILE 2 * idx + 1 < n DO
        minIdx = idx
        leftChildIdx = 2 * idx + 1
        rightChildIdx = 2 * idx + 2
        
        IF arr[leftChildIdx] < arr[minIdx] THEN
            minIdx = leftChildIdx
        END IF
        
        IF rightChildIdx < n AND arr[rightChildIdx] < arr[minIdx] THEN
            minIdx = rightChildIdx
        END IF
        
        IF minIdx != idx THEN
            SWAP(arr[idx], arr[minIdx])
            idx = minIdx
        ELSE
            BREAK
        END IF
    END WHILE
END FUNCTION
Complexity: O(n).
Note: More efficient than n push operations.

6. Heap Sort
text
FUNCTION heapSort(arr)
    n = arr.length
    
    // Build max-heap for ascending sort
    FOR i = (n / 2) - 1 DOWN TO 0 DO
        heapifyDownMaxHeap(arr, i, n)
    END FOR
    
    // Extract elements from heap
    FOR i = n - 1 DOWN TO 1 DO
        SWAP(arr[0], arr[i])
        heapifyDownMaxHeap(arr, 0, i)
    END FOR
    
    RETURN arr
END FUNCTION

FUNCTION heapifyDownMaxHeap(arr, idx, heapSize)
    WHILE 2 * idx + 1 < heapSize DO
        maxIdx = idx
        leftChildIdx = 2 * idx + 1
        rightChildIdx = 2 * idx + 2
        
        IF arr[leftChildIdx] > arr[maxIdx] THEN
            maxIdx = leftChildIdx
        END IF
        
        IF rightChildIdx < heapSize AND arr[rightChildIdx] > arr[maxIdx] THEN
            maxIdx = rightChildIdx
        END IF
        
        IF maxIdx != idx THEN
            SWAP(arr[idx], arr[maxIdx])
            idx = maxIdx
        ELSE
            BREAK
        END IF
    END WHILE
END FUNCTION
Complexity: O(n log n) guaranteed.

7. Max-Heap Implementation
text
CLASS MaxHeap
    arr = []
    
    FUNCTION push(val)
        arr.append(val)
        heapifyUp(arr.length - 1)
    END FUNCTION
    
    FUNCTION heapifyUp(idx)
        WHILE idx > 0 DO
            parentIdx = (idx - 1) / 2
            IF arr[idx] > arr[parentIdx] THEN
                SWAP(arr[idx], arr[parentIdx])
                idx = parentIdx
            ELSE
                BREAK
            END IF
        END WHILE
    END FUNCTION
    
    FUNCTION pop()
        IF arr.length == 0 THEN
            ERROR "Heap underflow"
        END IF
        maxVal = arr[0]
        arr[0] = arr[arr.length - 1]
        arr.removeLast()
        IF arr.length > 0 THEN
            heapifyDown(0)
        END IF
        RETURN maxVal
    END FUNCTION
    
    FUNCTION heapifyDown(idx)
        WHILE 2 * idx + 1 < arr.length DO
            maxIdx = idx
            leftChildIdx = 2 * idx + 1
            rightChildIdx = 2 * idx + 2
            
            IF arr[leftChildIdx] > arr[maxIdx] THEN
                maxIdx = leftChildIdx
            END IF
            
            IF rightChildIdx < arr.length AND arr[rightChildIdx] > arr[maxIdx] THEN
                maxIdx = rightChildIdx
            END IF
            
            IF maxIdx != idx THEN
                SWAP(arr[idx], arr[maxIdx])
                idx = maxIdx
            ELSE
                BREAK
            END IF
        END WHILE
    END FUNCTION
    
    FUNCTION peek()
        IF arr.length == 0 THEN
            ERROR "Heap empty"
        END IF
        RETURN arr[0]
    END FUNCTION
END CLASS
8. Kth Largest Element
text
FUNCTION findKthLargest(arr, k)
    minHeap = MinHeap()
    
    // Add first k elements to heap
    FOR i = 0 TO k - 1 DO
        minHeap.push(arr[i])
    END FOR
    
    // For each remaining element
    FOR i = k TO arr.length - 1 DO
        IF arr[i] > minHeap.peek() THEN
            minHeap.pop()
            minHeap.push(arr[i])
        END IF
    END FOR
    
    RETURN minHeap.peek()
END FUNCTION
Complexity: O(n log k).

9. Merge K Sorted Arrays
text
FUNCTION mergeKSortedArrays(arrays)
    minHeap = MinHeap()
    result = []
    
    // Initialize: add first element from each array
    FOR i = 0 TO arrays.length - 1 DO
        IF arrays[i].length > 0 THEN
            minHeap.push((arrays[i][0], i, 0))  // (value, arrayIndex, elementIndex)
        END IF
    END FOR
    
    // Extract elements in order
    WHILE NOT minHeap.isEmpty() DO
        val, arrayIdx, elemIdx = minHeap.pop()
        result.append(val)
        
        // Add next element from same array if exists
        IF elemIdx + 1 < arrays[arrayIdx].length THEN
            nextVal = arrays[arrayIdx][elemIdx + 1]
            minHeap.push((nextVal, arrayIdx, elemIdx + 1))
        END IF
    END WHILE
    
    RETURN result
END FUNCTION
Complexity: O(n log k) where n = total elements, k = number of arrays.

10. Median of Data Stream (Two Heaps)
text
CLASS MedianFinder
    maxHeapLeft = MaxHeap()   // Stores lower half
    minHeapRight = MinHeap()  // Stores upper half
    
    FUNCTION addNum(num)
        // Add to max-heap (left) if empty or num <= left max
        IF maxHeapLeft.isEmpty() OR num <= maxHeapLeft.peek() THEN
            maxHeapLeft.push(num)
        ELSE
            minHeapRight.push(num)
        END IF
        
        // Balance: left can have at most 1 more element
        IF maxHeapLeft.size() > minHeapRight.size() + 1 THEN
            val = maxHeapLeft.pop()
            minHeapRight.push(val)
        END IF
        
        IF minHeapRight.size() > maxHeapLeft.size() THEN
            val = minHeapRight.pop()
            maxHeapLeft.push(val)
        END IF
    END FUNCTION
    
    FUNCTION findMedian()
        IF maxHeapLeft.size() > minHeapRight.size() THEN
            RETURN float(maxHeapLeft.peek())
        ELSE
            RETURN (maxHeapLeft.peek() + minHeapRight.peek()) / 2.0
        END IF
    END FUNCTION
END CLASS
Complexity: O(log n) per add, O(1) median.

11. Top K Frequent Elements
text
FUNCTION topKFrequent(arr, k)
    // Count frequencies
    freqMap = {}
    FOR num IN arr DO
        freqMap[num] = freqMap.get(num, 0) + 1
    END FOR
    
    // Min-heap of size k by frequency
    minHeap = MinHeap()
    
    FOR num, freq IN freqMap DO
        minHeap.push((freq, num))
        IF minHeap.size() > k THEN
            minHeap.pop()
        END IF
    END FOR
    
    // Extract result
    result = []
    WHILE NOT minHeap.isEmpty() DO
        freq, num = minHeap.pop()
        result.append(num)
    END WHILE
    
    RETURN result
END FUNCTION
Complexity: O(n log k).

12. Dijkstra's Algorithm with Min-Heap
text
FUNCTION dijkstra(graph, start)
    distances = {}
    FOR v IN graph.vertices DO
        distances[v] = INFINITY
    END FOR
    distances[start] = 0
    
    minHeap = MinHeap()
    minHeap.push((0, start))  // (distance, node)
    visited = SET()
    
    WHILE NOT minHeap.isEmpty() DO
        currDist, node = minHeap.pop()
        
        IF node IN visited THEN
            CONTINUE
        END IF
        visited.add(node)
        
        FOR neighbor, weight IN graph.getNeighbors(node) DO
            IF neighbor NOT IN visited THEN
                newDist = currDist + weight
                IF newDist < distances[neighbor] THEN
                    distances[neighbor] = newDist
                    minHeap.push((newDist, neighbor))
                END IF
            END IF
        END FOR
    END WHILE
    
    RETURN distances
END FUNCTION
Complexity: O((V + E) log V).

13. Huffman Coding with Min-Heap
text
CLASS HuffmanNode
    char, freq
    left, right
END CLASS

FUNCTION huffmanCoding(s)
    // Count frequencies
    freqMap = {}
    FOR char IN s DO
        freqMap[char] = freqMap.get(char, 0) + 1
    END FOR
    
    // Build min-heap
    minHeap = MinHeap()
    FOR char, freq IN freqMap DO
        node = HuffmanNode(char, freq, null, null)
        minHeap.push((freq, node))
    END FOR
    
    // Build Huffman tree
    WHILE minHeap.size() > 1 DO
        freq1, node1 = minHeap.pop()
        freq2, node2 = minHeap.pop()
        
        parent = HuffmanNode(null, freq1 + freq2, node1, node2)
        minHeap.push((freq1 + freq2, parent))
    END WHILE
    
    freq, root = minHeap.pop()
    
    // Generate codes
    codes = {}
    FUNCTION generateCodes(node, code)
        IF node == null THEN RETURN END IF
        IF node.char != null THEN
            codes[node.char] = code
        ELSE
            generateCodes(node.left, code + "0")
            generateCodes(node.right, code + "1")
        END IF
    END FUNCTION
    
    generateCodes(root, "")
    RETURN codes
END FUNCTION
Complexity: O(n log n).

14. Priority Queue Operations
text
CLASS PriorityQueue
    minHeap = MinHeap()
    
    FUNCTION enqueue(element, priority)
        minHeap.push((priority, element))
    END FUNCTION
    
    FUNCTION dequeue()
        IF minHeap.isEmpty() THEN
            ERROR "Queue empty"
        END IF
        priority, element = minHeap.pop()
        RETURN element
    END FUNCTION
    
    FUNCTION peek()
        IF minHeap.isEmpty() THEN
            ERROR "Queue empty"
        END IF
        priority, element = minHeap.peek()
        RETURN element
    END FUNCTION
    
    FUNCTION isEmpty()
        RETURN minHeap.isEmpty()
    END FUNCTION
    
    FUNCTION size()
        RETURN minHeap.size()
    END FUNCTION
END CLASS
15. Heap Decrease-Key (For Advanced Heaps)
text
FUNCTION decreaseKey(heap, idx, newVal)
    IF newVal > heap[idx] THEN
        ERROR "New value larger than current"
    END IF
    
    heap[idx] = newVal
    heapifyUp(idx)
END FUNCTION

FUNCTION increaseKey(heap, idx, newVal)
    IF newVal < heap[idx] THEN
        ERROR "New value smaller than current"
    END IF
    
    heap[idx] = newVal
    heapifyDown(idx)
END FUNCTION
16. D-ary Heap Operations
text
CLASS DaryHeap
    arr = []
    d = 3  // 3-ary heap
    
    FUNCTION parent(i)
        RETURN (i - 1) / d
    END FUNCTION
    
    FUNCTION child(i, j)  // j-th child (0-indexed)
        RETURN d * i + j + 1
    END FUNCTION
    
    FUNCTION push(val)
        arr.append(val)
        heapifyUp(arr.length - 1)
    END FUNCTION
    
    FUNCTION heapifyUp(idx)
        WHILE idx > 0 DO
            parentIdx = parent(idx)
            IF arr[idx] < arr[parentIdx] THEN
                SWAP(arr[idx], arr[parentIdx])
                idx = parentIdx
            ELSE
                BREAK
            END IF
        END WHILE
    END FUNCTION
    
    FUNCTION pop()
        IF arr.length == 0 THEN
            ERROR "Heap underflow"
        END IF
        minVal = arr[0]
        arr[0] = arr[arr.length - 1]
        arr.removeLast()
        IF arr.length > 0 THEN
            heapifyDown(0)
        END IF
        RETURN minVal
    END FUNCTION
    
    FUNCTION heapifyDown(idx)
        WHILE true DO
            minIdx = idx
            FOR j = 0 TO d - 1 DO
                childIdx = child(idx, j)
                IF childIdx < arr.length AND arr[childIdx] < arr[minIdx] THEN
                    minIdx = childIdx
                END IF
            END FOR
            IF minIdx != idx THEN
                SWAP(arr[idx], arr[minIdx])
                idx = minIdx
            ELSE
                BREAK
            END IF
        END WHILE
    END FUNCTION
END CLASS
Advantage: Lower height O(log_d n), better cache locality for small d.

17. Pairing Heap Operations (Advanced)
text
CLASS PairingHeapNode
    value
    child
    sibling
END CLASS

CLASS PairingHeap
    root = null
    
    FUNCTION push(val)
        newNode = PairingHeapNode(val)
        IF root == null THEN
            root = newNode
        ELSE
            root = merge(root, newNode)
        END IF
    END FUNCTION
    
    FUNCTION merge(h1, h2)
        IF h1 == null THEN RETURN h2 END IF
        IF h2 == null THEN RETURN h1 END IF
        
        IF h1.value <= h2.value THEN
            h2.sibling = h1.child
            h1.child = h2
            RETURN h1
        ELSE
            h1.sibling = h2.child
            h2.child = h1
            RETURN h2
        END IF
    END FUNCTION
    
    FUNCTION pop()
        IF root == null THEN
            ERROR "Heap empty"
        END IF
        val = root.value
        root = mergePairs(root.child)
        RETURN val
    END FUNCTION
    
    FUNCTION mergePairs(node)
        IF node == null OR node.sibling == null THEN
            RETURN node
        END IF
        
        next = node.sibling.sibling
        RETURN merge(merge(node, node.sibling), mergePairs(next))
    END FUNCTION
END CLASS
Complexity: O(1) amortized push, O(log n) pop.

These pseudocode implementations provide the foundation for implementing heaps in any programming language and solving heap-based problems efficiently!