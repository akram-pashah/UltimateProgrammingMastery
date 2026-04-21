🟢 Pseudocode: Linked Lists – From Basics to Complex Patterns

1. Basic Node Structure
   text
   CLASS Node
   value
   next
   END CLASS
2. Insert at Head
   text
   FUNCTION insertAtHead(head, value)
   newNode = Node(value)
   newNode.next = head
   RETURN newNode
   END FUNCTION
3. Insert at Tail
   text
   FUNCTION insertAtTail(head, value)
   IF head == null THEN
   RETURN Node(value)
   END IF
   curr = head
   WHILE curr.next != null DO
   curr = curr.next
   END WHILE
   curr.next = Node(value)
   RETURN head
   END FUNCTION
4. Traverse and Print List
   text
   FUNCTION printList(head)
   curr = head
   WHILE curr != null DO
   PRINT curr.value
   curr = curr.next
   END WHILE
   END FUNCTION
5. Search Value in List
   text
   FUNCTION find(head, target)
   curr = head
   WHILE curr != null DO
   IF curr.value == target THEN
   RETURN true
   END IF
   curr = curr.next
   END WHILE
   RETURN false
   END FUNCTION
6. Delete Node by Value
   text
   FUNCTION deleteByValue(head, value)
   dummy = Node(0)
   dummy.next = head
   prev, curr = dummy, head
   WHILE curr != null DO
   IF curr.value == value THEN
   prev.next = curr.next
   BREAK
   END IF
   prev = curr
   curr = curr.next
   END WHILE
   RETURN dummy.next
   END FUNCTION
7. Reverse Linked List (Classic)
   text
   FUNCTION reverseList(head)
   prev = null
   curr = head
   WHILE curr != null DO
   nextNode = curr.next
   curr.next = prev
   prev = curr
   curr = nextNode
   END WHILE
   RETURN prev
   END FUNCTION
8. Detect Cycle (Floyd’s Tortoise & Hare)
   text
   FUNCTION hasCycle(head)
   slow, fast = head, head
   WHILE fast != null AND fast.next != null DO
   slow = slow.next
   fast = fast.next.next
   IF slow == fast THEN
   RETURN true
   END IF
   END WHILE
   RETURN false
   END FUNCTION
9. Find Intersection Node of Two Lists
   text
   FUNCTION getIntersection(headA, headB)
   a, b = headA, headB
   WHILE a != b DO
   a = (a == null) ? headB : a.next
   b = (b == null) ? headA : b.next
   END WHILE
   RETURN a
   END FUNCTION
10. Remove Nth Node from End (Two Pointer)
    text
    FUNCTION removeNthFromEnd(head, n)
    dummy = Node(0)
    dummy.next = head
    fast, slow = dummy, dummy
    FOR i = 1 TO n+1 DO
    fast = fast.next
    END FOR
    WHILE fast != null DO
    fast = fast.next
    slow = slow.next
    END WHILE
    slow.next = slow.next.next
    RETURN dummy.next
    END FUNCTION
11. Merge Two Sorted Linked Lists
    text
    FUNCTION mergeSortedLists(l1, l2)
    dummy = Node(0)
    curr = dummy
    WHILE l1 != null AND l2 != null DO
    IF l1.value < l2.value THEN
    curr.next = l1
    l1 = l1.next
    ELSE
    curr.next = l2
    l2 = l2.next
    END IF
    curr = curr.next
    END WHILE
    curr.next = (l1 != null) ? l1 : l2
    RETURN dummy.next
    END FUNCTION
12. Doubly Linked List Node
    text
    CLASS DNode
    value
    prev
    next
    END CLASS
13. Reverse Doubly Linked List
    text
    FUNCTION reverseDoublyList(head)
    curr = head
    temp = null
    WHILE curr != null DO
    temp = curr.prev
    curr.prev = curr.next
    curr.next = temp
    curr = curr.prev
    END WHILE
    IF temp != null THEN
    RETURN temp.prev
    ELSE
    RETURN null
    END IF
    END FUNCTION
    These patterns and procedures cover every foundational and advanced usage for linked lists in both interviews and robust production software development.
