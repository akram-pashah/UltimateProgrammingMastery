🟢 Examples: Stacks & Queues – Essential, Advanced, and Industry Patterns

1.  Basic Stack Operations
    text
    // Create Stack
    stack = new Stack()
    // Push
    stack.push(13)
    stack.push(8)
    stack.push(21) // stack: [13, 8, 21]
    // Pop
    topVal = stack.pop() // Returns 21, stack: [13,8]
    // Peek
    topNow = stack.peek() // Returns 8
    // IsEmpty
    isEmpty = stack.isEmpty() // Returns false
2.  Basic Queue Operations
    text
    // Create Queue
    queue = new Queue()
    // Enqueue
    queue.enqueue('X')
    queue.enqueue('Y')
    queue.enqueue('Z') // queue: front-> X, Y, Z
    // Dequeue
    first = queue.dequeue() // Returns 'X', queue: front-> Y, Z
    // Peek/Front
    currentFront = queue.peek() // Returns 'Y'
    // IsEmpty
    empty = queue.isEmpty() // false
3.  Balanced Parentheses (Stack Classic, FAANG)
    text
    function isValidParens(s):
    stack = []
    for char in s:
    if char in '([{':
    stack.push(char)
    else:
    if stack.isEmpty(): return false
    top = stack.pop()
    if not matching(top, char): return false
    return stack.isEmpty()
    Interview: Asked at Google, Amazon, Facebook

4.  Reverse a String Using Stack
    text
    function reverseString(s):
    stack = []
    for char in s:
    stack.push(char)
    output = ""
    while not stack.isEmpty():
    output += stack.pop()
    return output
    Real-world: Used in text editors, syntax parsers.

5.  Implement Queue Using Two Stacks
    text
    class MyQueue:
    stackIn = [], stackOut = []

        function enqueue(val):
            stackIn.push(val)
        function dequeue():
            if stackOut.isEmpty():
                while not stackIn.isEmpty():
                    stackOut.push(stackIn.pop())
            return stackOut.pop()

    FAANG Pattern: Understanding stack and queue synergy.

6.  Implement Stack With Two Queues
    text
    class MyStack:
    q1 = Queue(), q2 = Queue()

        function push(x):
            q2.enqueue(x)
            while not q1.isEmpty():
                q2.enqueue(q1.dequeue())
            swap q1, q2
        function pop():
            return q1.dequeue()

    Interview Challenge: Microsoft, Meta

7.  Min Stack (Constant-time min)
    text
    class MinStack:
    stack = []
    minStack = []

        function push(x):
            stack.push(x)
            if minStack.isEmpty() or x <= minStack.peek():
                minStack.push(x)
        function pop():
            if stack.pop() == minStack.peek():
                minStack.pop()
        function getMin():
            return minStack.peek()

    Leetcode: Frequently asked.

8.  Sliding Window Maximum (Deque/Queue Pattern)
    text
    function slidingWindowMax(nums, k):
    dq = Deque()
    result = []
    for i in 0 to nums.length-1:
    while dq and nums[i] > nums[dq[-1]]:
    dq.popBack()
    dq.pushBack(i)
    if dq[0] == i-k:
    dq.popFront()
    if i >= k-1:
    result.append(nums[dq[0]])
    return result
    Used in: Streaming analytics, Top K in finance, IoT.

9.  Producer-Consumer Queue (Classic OS/Concurrency)
    text
    queue = new Queue()
    function producer():
    while producing:
    data = makeData()
    queue.enqueue(data)

function consumer():
while consuming:
if not queue.isEmpty():
data = queue.dequeue()
process(data)
Industry: OS, distributed systems (Kafka, RabbitMQ), real-time messaging.

10. Browser Back/Forward (Deque)
    text
    backStack = Stack()
    forwardStack = Stack()
    function visit(page):
    backStack.push(page)
    forwardStack.clear()
    function back():
    if not backStack.isEmpty():
    page = backStack.pop()
    forwardStack.push(page)
    function forward():
    if not forwardStack.isEmpty():
    page = forwardStack.pop()
    backStack.push(page)
    Real-World: Chrome, Firefox navigation models.

11. Level Order Traversal (Queue, BFS on Trees/Graphs)
    text
    function levelOrder(root):
    queue = Queue()
    queue.enqueue(root)
    while not queue.isEmpty():
    node = queue.dequeue()
    process(node)
    for child in node.children:
    queue.enqueue(child)
    Used in: Graph traversals, AI (state exploration), real-world logistics.

12. Simulating Undo/Redo with Two Stacks
    text
    undoStack = Stack()
    redoStack = Stack()
    function doAction(action):
    undoStack.push(action)
    redoStack.clear()
    function undo():
    if not undoStack.isEmpty():
    redoStack.push(undoStack.pop())
    function redo():
    if not redoStack.isEmpty():
    undoStack.push(redoStack.pop())
    Real-World: Google Docs, Photoshop, VSCode.

13. Circular Queue for Efficient Buffering
    text
    class CircularQueue:
    arr, head, tail, size
    function enqueue(val):
    if full: error
    arr[tail] = val
    tail = (tail+1) % arr.length
    function dequeue():
    if empty: error
    val = arr[head]
    head = (head+1) % arr.length
    return val
    Applications: Real-time streaming, gaming, embedded systems.

Master these examples to unlock powerful, efficient logic and reasoning for both coding interviews and production-grade design!
