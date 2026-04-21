🟢 Examples: Linked Lists – Practical, Interview, and Product Patterns

1. Basic Singly Linked List Construction
   text
   class Node:
   value
   next

// Create nodes
node1 = Node(7)
node2 = Node(12)
node3 = Node(20)

// Link nodes
node1.next = node2
node2.next = node3

// Traverse
current = node1
while current != null:
print(current.value)
current = current.next 2. Insert at Head (O(1) Operation)
text
function insertAtHead(head, value):
newNode = Node(value)
newNode.next = head
return newNode
Usage: Fast prepend for task logs, buffer, undo stack.

3. Insert at Tail (with Tail Pointer, O(1))
   text
   function insertAtTail(tail, value):
   newNode = Node(value)
   tail.next = newNode
   return newNode
   Optimized for: Batch updates, appends in networking/database logs.

4. Reverse a Singly Linked List (FAANG Classic)
   text
   function reverseList(head):
   prev = null
   current = head
   while current != null:
   nextNode = current.next
   current.next = prev
   prev = current
   current = nextNode
   return prev
   Common at: Amazon, Google, Facebook interviews.

5. Detect Cycle (Floyd’s Tortoise & Hare Algorithm)
   text
   function hasCycle(head):
   slow = head
   fast = head
   while fast != null and fast.next != null:
   slow = slow.next
   fast = fast.next.next
   if slow == fast:
   return true
   return false
   Real-World: Find infinite loops, buffer mislinks.

6. Find Intersection of Two Linked Lists
   text
   function getIntersectionNode(headA, headB):
   currA = headA
   currB = headB
   while currA != currB:
   currA = currA == null ? headB : currA.next
   currB = currB == null ? headA : currB.next
   return currA
   Why: Used for merging user actions, social networks, edit histories.

7. Remove Nth Node from End
   text
   function removeNthFromEnd(head, n):
   dummy = Node(0)
   dummy.next = head
   fast, slow = dummy, dummy
   for i = 1 to n+1:
   fast = fast.next
   while fast != null:
   fast = fast.next
   slow = slow.next
   slow.next = slow.next.next
   return dummy.next
   Patterns: Clean deletion with two pointers.

8. Merge Two Sorted Lists
   text
   function mergeSortedLists(l1, l2):
   dummy = Node(0)
   curr = dummy
   while l1 != null and l2 != null:
   if l1.value < l2.value:
   curr.next = l1
   l1 = l1.next
   else:
   curr.next = l2
   l2 = l2.next
   curr = curr.next
   curr.next = l1 != null ? l1 : l2
   return dummy.next
   Used in: Database merge, file diffs, event logs.

9. Doubly Linked List – Basic Usage
   text
   class DNode:
   value
   prev
   next

nodeA = DNode(1)
nodeB = DNode(2)
nodeA.next = nodeB
nodeB.prev = nodeA
Can reverse traverse, efficient for LRU cache.

10. LRU Cache Skeleton Using Doubly Linked List + HashMap (FAANG)
    text
    class LRUCache:
    map = {}
    head, tail = DNode(), DNode()

        function get(key):
            if key not in map:
                return -1
            node = map[key]
            remove(node)
            insertAtHead(node)
            return node.value

        function put(key, value):
            if key in map:
                remove(map[key])
            node = DNode(key, value)
            insertAtHead(node)
            map[key] = node
            if map.size > capacity:
                remove(tail.prev)   // Remove LRU node
                map.remove(tail.prev.key)

    Industry: OS page management, browser tabs, Redis/DB cache.

11. Skip List Overview (High Perf DS)
    text
    class SkipNode:
    value
    next // Next pointer
    down // Points to node on lower level

// Used for O(log n) search in ordered lists, LevelDB, Redis, advanced DBs 12. Cycle Creation Pattern (For Testing)
text
node1 = Node(1)
node2 = Node(2)
node3 = Node(3)
node1.next = node2
node2.next = node3
node3.next = node1 // Creates a cycle 13. Real-World: Browser Back/Forward Navigation
text
class NavigationHistory:
head = null
curr = null

    function visitPage(url):
        newNode = Node(url)
        if curr != null:
            newNode.prev = curr
            curr.next = newNode
        curr = newNode

Allows O(1) forward/back jumps using doubly linked list.

14. Undo/Redo Stack for Editor
    text
    class CommandNode:
    command
    next
    prev

// Undo: move to prev
// Redo: move to next

Supports multi-level undo/redo history.

15. Interview Traps
    Reversing with head/tail confusion.

Removing first/last node edge cases.

Correct cycle detection.

Proper memory clean-up for all removed nodes.

Practice these examples to own classic patterns, system-level skills, and interview success for any environment—from engineering boards to real-world product features!
