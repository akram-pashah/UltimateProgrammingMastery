🟢 Pseudocode: Stacks & Queues – Operations, Patterns, and Interview Logic

1. Stack Structure and Operations
   text
   CLASS Stack
   arr = []
   FUNCTION push(x)
   arr.append(x)
   END FUNCTION
   FUNCTION pop()
   IF arr.length == 0 THEN
   ERROR "Stack Underflow"
   END IF
   RETURN arr.pop()
   END FUNCTION
   FUNCTION peek()
   IF arr.length == 0 THEN
   ERROR "Empty Stack"
   END IF
   RETURN arr[arr.length-1]
   END FUNCTION
   FUNCTION isEmpty()
   RETURN arr.length == 0
   END FUNCTION
   END CLASS
2. Queue Structure and Operations
   text
   CLASS Queue
   arr = []
   FUNCTION enqueue(x)
   arr.append(x)
   END FUNCTION
   FUNCTION dequeue()
   IF arr.length == 0 THEN
   ERROR "Queue Underflow"
   END IF
   RETURN arr.pop(0)
   END FUNCTION
   FUNCTION peek()
   IF arr.length == 0 THEN
   ERROR "Empty Queue"
   END IF
   RETURN arr[0]
   END FUNCTION
   FUNCTION isEmpty()
   RETURN arr.length == 0
   END FUNCTION
   END CLASS
3. Implement Stack Using Two Queues
   text
   CLASS MyStack
   q1 = Queue()
   q2 = Queue()
   FUNCTION push(x)
   q2.enqueue(x)
   WHILE NOT q1.isEmpty() DO
   q2.enqueue(q1.dequeue())
   END WHILE
   SWAP(q1, q2)
   END FUNCTION
   FUNCTION pop()
   RETURN q1.dequeue()
   END FUNCTION
   END CLASS
4. Implement Queue Using Two Stacks
   text
   CLASS MyQueue
   stackIn = Stack()
   stackOut = Stack()
   FUNCTION enqueue(x)
   stackIn.push(x)
   END FUNCTION
   FUNCTION dequeue()
   IF stackOut.isEmpty() THEN
   WHILE NOT stackIn.isEmpty() DO
   stackOut.push(stackIn.pop())
   END WHILE
   END IF
   RETURN stackOut.pop()
   END FUNCTION
   END CLASS
5. Min Stack (Tracking Minimum in O(1) Time)
   text
   CLASS MinStack
   stack = []
   minStack = []
   FUNCTION push(x)
   stack.append(x)
   IF minStack.isEmpty() OR x <= minStack[-1] THEN
   minStack.append(x)
   END IF
   END FUNCTION
   FUNCTION pop()
   val = stack.pop()
   IF val == minStack[-1] THEN
   minStack.pop()
   END IF
   END FUNCTION
   FUNCTION getMin()
   RETURN minStack[-1]
   END FUNCTION
   END CLASS
6. Producer-Consumer Queue Pattern
   text
   queue = Queue()

FUNCTION producer():
WHILE producing:
data = makeData()
queue.enqueue(data)

FUNCTION consumer():
WHILE consuming:
IF NOT queue.isEmpty() THEN
data = queue.dequeue()
process(data) 7. Sliding Window Maximum (Deque Pattern)
text
FUNCTION slidingWindowMax(nums, k):
dq = Deque() // stores indices
result = []
FOR i = 0 TO nums.length-1 DO
WHILE NOT dq.isEmpty() AND nums[i] > nums[dq[-1]] DO
dq.popBack()
END WHILE
dq.pushBack(i)
IF dq[0] == i - k THEN
dq.popFront()
END IF
IF i >= k-1 THEN
result.append(nums[dq[0]])
END FOR
RETURN result
END FUNCTION 8. Level Order Traversal (BFS using Queue)
text
FUNCTION levelOrder(root):
queue = Queue()
queue.enqueue(root)
WHILE NOT queue.isEmpty() DO
node = queue.dequeue()
process(node)
FOR child IN node.children DO
queue.enqueue(child)
END WHILE
END FUNCTION 9. Undo/Redo Logic Using Two Stacks
text
undoStack = Stack()
redoStack = Stack()

FUNCTION doAction(action):
undoStack.push(action)
redoStack.clear()

FUNCTION undo():
IF NOT undoStack.isEmpty() THEN
redoStack.push(undoStack.pop())

FUNCTION redo():
IF NOT redoStack.isEmpty() THEN
undoStack.push(redoStack.pop()) 10. Circular Queue (Ring Buffer)
text
CLASS CircularQueue
arr
head
tail
size

    FUNCTION enqueue(x)
        IF (tail + 1) % length(arr) == head THEN
            ERROR "Queue Full"
        END IF
        arr[tail] = x
        tail = (tail + 1) % length(arr)
    END FUNCTION

    FUNCTION dequeue()
        IF head == tail THEN
            ERROR "Queue Empty"
        END IF
        x = arr[head]
        head = (head + 1) % length(arr)
        RETURN x
    END FUNCTION

END CLASS
These pseudocode patterns provide a direct path to mastering stacks and queues for both interviews and top-quality systems engineering.
