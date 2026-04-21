🟢 Interactive Logging: Stacks & Queues – Step-by-Step Operation Traces

1. Stack Push/Pop/Peek Example
   text
   [Create] Stack s
   [Push] s.push(10) // Stack: [10]
   [Push] s.push(23) // Stack: [10, 23]
   [Push] s.push(5) // Stack: [10, 23, 5]
   [Peek] s.peek() // Output: 5
   [Pop] s.pop() // Output: 5, Stack: [10, 23]
   [Pop] s.pop() // Output: 23, Stack: [10]
2. Queue Enqueue/Dequeue/Peek Example
   text
   [Create] Queue q
   [Enqueue] q.enqueue('A') // Queue: ['A']
   [Enqueue] q.enqueue('B') // Queue: ['A', 'B']
   [Enqueue] q.enqueue('C') // Queue: ['A', 'B', 'C']
   [Peek] q.peek() // Output: 'A'
   [Dequeue] q.dequeue() // Output: 'A', Queue: ['B', 'C']
   [Dequeue] q.dequeue() // Output: 'B', Queue: ['C']
3. Balanced Parentheses (Stack Trace)
   text
   [Input] s = "{[(])}"
   [Step] Push '{' → Stack: ['{']
   [Step] Push '[' → Stack: ['{','[']
   [Step] Push '(' → Stack: ['{','[','(']
   [Step] Check ']': Top is '(' (Mismatch: expected ')')
   [Result] Invalid parentheses (Early exit)
4. Reverse String (Stack Trace)
   text
   [Input] s = "CODE"
   [Push] 'C' → ['C']
   [Push] 'O' → ['C','O']
   [Push] 'D' → ['C','O','D']
   [Push] 'E' → ['C','O','D','E']
   [Pop] 'E', Output: "E"
   [Pop] 'D', Output: "ED"
   [Pop] 'O', Output: "EDO"
   [Pop] 'C', Output: "EDOC"
5. Min Stack (O(1) Minimum Trace)
   text
   [Push] 8 → MinStack: [8], Stack: [8]
   [Push] 4 → MinStack: [8,4], Stack: [8,4]
   [getMin] → Output: 4
   [Push] 5 → MinStack: [8,4], Stack: [8,4,5]
   [getMin] → Output: 4
   [Pop] 5 → MinStack: [8,4], Stack: [8,4]
   [Pop] 4 → MinStack: [8], Stack: [8]
   [getMin] → Output: 8
6. Implement Queue with Two Stacks (Trace)
   text
   [Enqueue] 1 → stackIn: [1], stackOut: []
   [Enqueue] 2 → stackIn: [1,2], stackOut: []
   [Dequeue] stackOut is empty → Move all from stackIn to stackOut
   Transfer: stackIn.pop() → stackOut.push(2), stackIn: [1], stackOut: [2]
   Transfer: stackIn.pop() → stackOut.push(1), stackIn: [], stackOut: [2,1]
   Pop: stackOut.pop() → Output: 1
   [Enqueue] 3 → stackIn: [3], stackOut: [2]
   [Dequeue] stackOut.pop() → Output: 2
7. Sliding Window Maximum (Deque Trace, k=3)
   text
   [Array] [9, 5, 7, 11, 3, 8], Deque = [], MaxResult = []

i=0: push 0 → Deque=[0]
i=1: nums[1]=5 < nums[0]=9 → push 1 → Deque=[0,1]
i=2: nums[2]=7 > nums[1]=5 → pop 1, push 2 → Deque=[0,2]
[Window Full: i=2] MaxResult: nums[0]=9

i=3: nums[3]=11 > nums[2]=7 → pop 2, nums[3]=11 > nums[0]=9 → pop 0, push 3 → Deque=[3]
[Window Full: i=3] MaxResult: nums[3]=11

i=4: push 4 → Deque=[3,4]
[Window Full: i=4] MaxResult: nums[3]=11

i=5: nums[5]=8 < nums[4]=3 → push 5 → Deque=[3,4,5]
[Window Full: i=5] MaxResult: nums[3]=11

[Result] MaxResult: [9,11,11,11] 8. Producer-Consumer Scenario (Queue Trace)
text
[Producer] makes "item1", enqueue → Queue: ['item1']
[Consumer] dequeues "item1", process
[Producer] makes "item2", enqueue → Queue: ['item2']
[Producer] makes "item3", enqueue → Queue: ['item2','item3']
[Consumer] dequeues "item2", process
[Consumer] dequeues "item3", process
[Queue Empty] consumer waits... 9. Undo/Redo Trace (Text Editor, Two Stacks)
text
[Do] type 'A' → undoStack: ['A'], redoStack: []
[Do] type 'B' → undoStack: ['A','B'], redoStack: []
[Undo] pop 'B' → undoStack: ['A'], redoStack: ['B']
[Undo] pop 'A' → undoStack: [], redoStack: ['B','A']
[Redo] pop 'A' → redoStack: ['B'], undoStack: ['A']
[Redo] pop 'B' → redoStack: [], undoStack: ['A','B'] 10. Circular Queue Trace
text
[Create] size=3, head=0, tail=0
[Enqueue] 1 → arr=[1,_,_], tail=1
[Enqueue] 2 → arr=[1,2,_], tail=2
[Enqueue] 3 → arr=[1,2,3], tail=0 (wraps around)
[Dequeue] → Output: 1, head=1
[Enqueue] 4 → arr=[4,2,3], tail=1
These interactive logs illuminate how stacks and queues operate, morph, and solve real problems—giving you deep intuition for both interviews and software design.
