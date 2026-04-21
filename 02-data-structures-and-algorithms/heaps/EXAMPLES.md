🟢 Examples: Heaps – Essential, Advanced, and FAANG-Level Mastery
1. Basic Min-Heap Operations
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
    
    FUNCTION push(val)
        arr.append(val)
        heapifyUp(arr.length - 1)
    END FUNCTION
    
    FUNCTION heapifyUp(idx)
        WHILE idx > 0 AND arr[parent(idx)] > arr[idx] DO
            SWAP(arr[idx], arr[parent(idx)])
            idx = parent(idx)
        END WHILE
    END FUNCTION
    
    FUNCTION pop()
        IF arr.length == 0 THEN
            ERROR "Heap empty"
        END IF
        root = arr[0]
        arr[0] = arr[arr.length - 1]
        arr.removeLast()
        heapifyDown(0)
        RETURN root
    END FUNCTION
    
    FUNCTION heapifyDown(idx)
        WHILE 2 * idx + 1 < arr.length DO
            minIdx = idx
            IF arr[2 * idx + 1] < arr[minIdx] THEN
                minIdx = 2 * idx + 1
            END IF
            IF 2 * idx + 2 < arr.length AND arr[2 * idx + 2] < arr[minIdx] THEN
                minIdx = 2 * idx + 2
            END IF
            IF minIdx == idx THEN
                BREAK
            END IF
            SWAP(arr[idx], arr[minIdx])
            idx = minIdx
        END WHILE
    END FUNCTION
    
    FUNCTION peek()
        IF arr.length == 0 THEN
            ERROR "Heap empty"
        END IF
        RETURN arr[0]
    END FUNCTION
END CLASS
2. Build Heap from Array (Heapify)
text
FUNCTION buildHeap(arr)
    // Start from last non-leaf node and heapify down
    FOR i = (arr.length / 2) - 1 DOWN TO 0 DO
        heapifyDown(arr, i, arr.length)
    END FOR
    RETURN arr
END FUNCTION

FUNCTION heapifyDown(arr, idx, n)
    WHILE 2 * idx + 1 < n DO
        minIdx = idx
        leftChild = 2 * idx + 1
        rightChild = 2 * idx + 2
        
        IF arr[leftChild] < arr[minIdx] THEN
            minIdx = leftChild
        END IF
        IF rightChild < n AND arr[rightChild] < arr[minIdx] THEN
            minIdx = rightChild
        END IF
        IF minIdx == idx THEN
            BREAK
        END IF
        SWAP(arr[idx], arr[minIdx])
        idx = minIdx
    END WHILE
END FUNCTION
Complexity: O(n) time (not O(n log n)).
Used in: Heap sort, building priority queues from initial data.

3. Heap Sort
text
FUNCTION heapSort(arr)
    n = arr.length
    
    // Build heap
    FOR i = (n / 2) - 1 DOWN TO 0 DO
        heapifyDown(arr, i, n)
    END FOR
    
    // Extract elements one by one
    FOR i = n - 1 DOWN TO 1 DO
        SWAP(arr[0], arr[i])
        heapifyDown(arr, 0, i)
    END FOR
    
    RETURN arr
END FUNCTION
Complexity: O(n log n) guaranteed.
Used in: When O(log n) worst-case required; predictable performance.
Interviewed at: All FAANG.

4. Kth Largest Element (Min-Heap of Size K)
text
FUNCTION findKthLargest(arr, k)
    minHeap = MinHeap()
    
    // Add first k elements
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
Interviewed at: Google, Amazon, Facebook, Apple.
Use: Top-K largest in stream, online algorithm.

5. Top K Frequent Elements
text
FUNCTION topKFrequent(arr, k)
    // Count frequencies
    freqMap = {}
    FOR num IN arr DO
        freqMap[num] = freqMap.get(num, 0) + 1
    END FOR
    
    // Min-heap of size k by frequency
    minHeap = MinHeap()  // By frequency
    
    FOR num, freq IN freqMap DO
        minHeap.push((freq, num))
        IF minHeap.length > k THEN
            minHeap.pop()
        END IF
    END FOR
    
    result = []
    WHILE NOT minHeap.isEmpty() DO
        freq, num = minHeap.pop()
        result.append(num)
    END WHILE
    
    RETURN result
END FUNCTION
Complexity: O(n log k).
Interviewed at: All FAANG.

6. Kth Smallest Element (Max-Heap of Size K)
text
FUNCTION findKthSmallest(arr, k)
    maxHeap = MaxHeap()
    
    FOR i = 0 TO k - 1 DO
        maxHeap.push(arr[i])
    END FOR
    
    FOR i = k TO arr.length - 1 DO
        IF arr[i] < maxHeap.peek() THEN
            maxHeap.pop()
            maxHeap.push(arr[i])
        END IF
    END FOR
    
    RETURN maxHeap.peek()
END FUNCTION
Complexity: O(n log k).

7. Median of Data Stream (Two Heaps)
text
CLASS MedianFinder
    maxHeapLeft = MaxHeap()   // Lower half
    minHeapRight = MinHeap()  // Upper half
    
    FUNCTION addNum(num)
        // Balance: left has at most 1 more than right
        IF maxHeapLeft.isEmpty() OR num <= maxHeapLeft.peek() THEN
            maxHeapLeft.push(num)
        ELSE
            minHeapRight.push(num)
        END IF
        
        // Rebalance if needed
        IF maxHeapLeft.length > minHeapRight.length + 1 THEN
            val = maxHeapLeft.pop()
            minHeapRight.push(val)
        END IF
        IF minHeapRight.length > maxHeapLeft.length THEN
            val = minHeapRight.pop()
            maxHeapLeft.push(val)
        END IF
    END FUNCTION
    
    FUNCTION findMedian()
        IF maxHeapLeft.length > minHeapRight.length THEN
            RETURN float(maxHeapLeft.peek())
        ELSE
            RETURN (maxHeapLeft.peek() + minHeapRight.peek()) / 2.0
        END IF
    END FUNCTION
END CLASS
Complexity: O(log n) per add, O(1) per median.
Interviewed at: Google, Microsoft, Amazon.

8. Merge K Sorted Lists (Min-Heap)
text
CLASS ListNode
    val, next
END CLASS

FUNCTION mergeKLists(lists)
    minHeap = MinHeap()
    
    // Add first node from each list
    FOR listNode IN lists DO
        IF listNode != null THEN
            minHeap.push((listNode.val, listNode))
        END IF
    END FOR
    
    dummy = ListNode(0)
    curr = dummy
    
    WHILE NOT minHeap.isEmpty() DO
        val, node = minHeap.pop()
        curr.next = node
        curr = curr.next
        
        IF node.next != null THEN
            minHeap.push((node.next.val, node.next))
        END IF
    END WHILE
    
    RETURN dummy.next
END FUNCTION
Complexity: O(n log k) where n = total nodes, k = number of lists.
Interviewed at: Google, Amazon, Facebook.

9. Reorganize String (Max-Heap)
text
FUNCTION reorganizeString(s)
    // Count character frequencies
    freqMap = {}
    FOR char IN s DO
        freqMap[char] = freqMap.get(char, 0) + 1
    END FOR
    
    // Max-heap by frequency
    maxHeap = MaxHeap()
    FOR char, freq IN freqMap DO
        maxHeap.push((freq, char))
    END FOR
    
    result = []
    WHILE maxHeap.length > 0 DO
        // Take two most frequent characters
        freq1, char1 = maxHeap.pop()
        
        IF result.isEmpty() OR result[-1] != char1 THEN
            result.append(char1)
            IF freq1 > 1 THEN
                maxHeap.push((freq1 - 1, char1))
            END IF
        ELSE IF NOT maxHeap.isEmpty() THEN
            freq2, char2 = maxHeap.pop()
            result.append(char2)
            IF freq2 > 1 THEN
                maxHeap.push((freq2 - 1, char2))
            END IF
            maxHeap.push((freq1, char1))
        ELSE
            RETURN ""  // Impossible
        END IF
    END WHILE
    
    RETURN result.join()
END FUNCTION
Complexity: O(n log n).
Interviewed at: Google, Facebook.

10. Sliding Window Maximum (Deque + Heap Hybrid)
text
FUNCTION maxSlidingWindow(arr, k)
    result = []
    maxHeap = MaxHeap()  // Or use deque for O(1) operations
    
    FOR i = 0 TO arr.length - 1 DO
        // Add current element
        maxHeap.push(arr[i])
        
        // Remove elements outside window
        WHILE NOT maxHeap.isEmpty() AND maxHeap.peek() < i - k + 1 DO
            maxHeap.pop()
        END WHILE
        
        // Record max of current window
        IF i >= k - 1 THEN
            result.append(maxHeap.peek())
        END IF
    END FOR
    
    RETURN result
END FUNCTION
Complexity: O(n log n) with heap (O(n) with optimized deque).
Interviewed at: Google, Amazon, Facebook.

11. Dijkstra's Algorithm (Min-Heap)
text
FUNCTION dijkstra(graph, start)
    distances = {}
    FOR v IN graph.vertices DO
        distances[v] = INFINITY
    END FOR
    distances[start] = 0
    
    minHeap = MinHeap()
    minHeap.push((0, start))
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
Used in: GPS navigation, network routing.
Interviewed at: All FAANG.

12. Prim's MST Algorithm (Min-Heap)
text
FUNCTION primMST(graph, start)
    inMST = SET()
    key = {}
    parent = {}
    
    FOR v IN graph.vertices DO
        key[v] = INFINITY
        parent[v] = null
    END FOR
    
    key[start] = 0
    minHeap = MinHeap()
    minHeap.push((0, start))
    
    WHILE NOT minHeap.isEmpty() DO
        weight, u = minHeap.pop()
        
        IF u IN inMST THEN
            CONTINUE
        END IF
        inMST.add(u)
        
        FOR v, edgeWeight IN graph.getNeighbors(u) DO
            IF v NOT IN inMST AND edgeWeight < key[v] THEN
                key[v] = edgeWeight
                parent[v] = u
                minHeap.push((edgeWeight, v))
            END IF
        END FOR
    END WHILE
    
    RETURN parent, key
END FUNCTION
Complexity: O(E log V).

13. Huffman Coding (Min-Heap)
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
    
    // Build min-heap of nodes
    minHeap = MinHeap()
    FOR char, freq IN freqMap DO
        node = HuffmanNode(char, freq)
        minHeap.push(node)
    END FOR
    
    // Build tree
    WHILE minHeap.length > 1 DO
        left = minHeap.pop()
        right = minHeap.pop()
        parent = HuffmanNode(null, left.freq + right.freq)
        parent.left = left
        parent.right = right
        minHeap.push(parent)
    END WHILE
    
    root = minHeap.pop()
    
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
Used in: Data compression, lossless encoding.

14. Reorganize Tasks / Task Scheduler
text
FUNCTION leastInterval(tasks, n)
    // Count task frequencies
    taskCount = {}
    FOR task IN tasks DO
        taskCount[task] = taskCount.get(task, 0) + 1
    END FOR
    
    // Max-heap by frequency
    maxHeap = MaxHeap()
    FOR task, count IN taskCount DO
        maxHeap.push(count)
    END FOR
    
    time = 0
    WHILE NOT maxHeap.isEmpty() DO
        cycle = []
        FOR i = 0 TO n DO  // n+1 time units per cycle
            IF NOT maxHeap.isEmpty() THEN
                count = maxHeap.pop()
                IF count > 1 THEN
                    cycle.append(count - 1)
                END IF
            END IF
            time += 1
            IF maxHeap.isEmpty() AND cycle.isEmpty() THEN
                BREAK
            END IF
        END FOR
        
        FOR count IN cycle DO
            maxHeap.push(count)
        END FOR
    END WHILE
    
    RETURN time
END FUNCTION
Complexity: O(n) where n = total tasks.
Interviewed at: Google, Facebook.

15. IPO (Capital Allocation with Greedy Heap)
text
FUNCTION findMaximizedCapital(k, w, profits, capital)
    n = len(profits)
    projects = []
    FOR i = 0 TO n - 1 DO
        projects.append((capital[i], profits[i]))
    END FOR
    
    SORT(projects BY capital requirement)
    
    maxHeap = MaxHeap()  // By profit
    idx = 0
    currentCapital = w
    
    FOR i = 0 TO k - 1 DO
        // Add all projects we can afford
        WHILE idx < n AND projects[idx][0] <= currentCapital DO
            maxHeap.push(projects[idx][1])
            idx += 1
        END WHILE
        
        IF maxHeap.isEmpty() THEN
            BREAK
        END IF
        
        // Take most profitable project
        maxProfit = maxHeap.pop()
        currentCapital += maxProfit
    END FOR
    
    RETURN currentCapital
END FUNCTION
Complexity: O(n log n).
Interviewed at: Google, Amazon.

16. Frequency Sort by Heap
text
FUNCTION frequencySort(s)
    freqMap = {}
    FOR char IN s DO
        freqMap[char] = freqMap.get(char, 0) + 1
    END FOR
    
    maxHeap = MaxHeap()
    FOR char, freq IN freqMap DO
        maxHeap.push((freq, char))
    END FOR
    
    result = []
    WHILE NOT maxHeap.isEmpty() DO
        freq, char = maxHeap.pop()
        FOR i = 0 TO freq - 1 DO
            result.append(char)
        END FOR
    END WHILE
    
    RETURN result.join()
END FUNCTION
Complexity: O(n log n).

These examples cover foundational to advanced heap patterns—master them for FAANG interviews, system design, and real-world engineering challenges!