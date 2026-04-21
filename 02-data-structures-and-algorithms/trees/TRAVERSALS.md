🟢 Tree Traversals: Methods, Patterns & Mastery
1. Overview of Tree Traversals
Tree traversals are systematic ways to visit every node exactly once. They are categorized into:

Depth-First Search (DFS): Explore deeply before backtracking.

Inorder, Preorder, Postorder (for binary trees)

Breadth-First Search (BFS): Explore level-by-level.

Level Order

2. Inorder Traversal (Left-Root-Right)
Use Case:
BST inorder yields sorted order.

Natural for printing balanced structures.

Recursive Implementation
text
FUNCTION inorderRecursive(node, result)
    IF node == null THEN
        RETURN
    END IF
    inorderRecursive(node.left, result)
    result.append(node.value)
    inorderRecursive(node.right, result)
END FUNCTION
Iterative Implementation (with Stack)
text
FUNCTION inorderIterative(root)
    stack = []
    result = []
    curr = root
    WHILE curr != null OR NOT stack.isEmpty() DO
        WHILE curr != null DO
            stack.push(curr)
            curr = curr.left
        END WHILE
        curr = stack.pop()
        result.append(curr.value)
        curr = curr.right
    END WHILE
    RETURN result
END FUNCTION
Complexity:
Time: O(n)

Space: O(h) where h = height (recursion stack or explicit stack)

3. Preorder Traversal (Root-Left-Right)
Use Case:
Serializing/copying tree.

Creating prefix notation.

Root must be processed first.

Recursive Implementation
text
FUNCTION preorderRecursive(node, result)
    IF node == null THEN
        RETURN
    END IF
    result.append(node.value)
    preorderRecursive(node.left, result)
    preorderRecursive(node.right, result)
END FUNCTION
Iterative Implementation (with Stack)
text
FUNCTION preorderIterative(root)
    stack = [root]
    result = []
    WHILE NOT stack.isEmpty() DO
        curr = stack.pop()
        result.append(curr.value)
        IF curr.right != null THEN
            stack.push(curr.right)
        END IF
        IF curr.left != null THEN
            stack.push(curr.left)
        END IF
    END WHILE
    RETURN result
END FUNCTION
Complexity:
Time: O(n)

Space: O(h)

4. Postorder Traversal (Left-Right-Root)
Use Case:
Deleting tree (process children before parent).

Postfix notation evaluation.

Computing tree properties (height, subtree size).

Recursive Implementation
text
FUNCTION postorderRecursive(node, result)
    IF node == null THEN
        RETURN
    END IF
    postorderRecursive(node.left, result)
    postorderRecursive(node.right, result)
    result.append(node.value)
END FUNCTION
Iterative Implementation (with Two Stacks)
text
FUNCTION postorderIterative(root)
    stack1 = [root]
    stack2 = []
    result = []
    WHILE NOT stack1.isEmpty() DO
        curr = stack1.pop()
        stack2.push(curr)
        IF curr.left != null THEN
            stack1.push(curr.left)
        END IF
        IF curr.right != null THEN
            stack1.push(curr.right)
        END IF
    END WHILE
    WHILE NOT stack2.isEmpty() DO
        result.append(stack2.pop().value)
    END WHILE
    RETURN result
END FUNCTION
Complexity:
Time: O(n)

Space: O(h)

5. Level Order Traversal (BFS)
Use Case:
Processing tree level-by-level.

Serializing tree to array (array-backed heap).

Finding nodes at specific depth.

Implementation (with Queue)
text
FUNCTION levelOrder(root)
    queue = Queue()
    result = []
    queue.enqueue(root)
    WHILE NOT queue.isEmpty() DO
        levelSize = queue.size()
        currentLevel = []
        FOR i = 1 TO levelSize DO
            node = queue.dequeue()
            currentLevel.append(node.value)
            IF node.left != null THEN
                queue.enqueue(node.left)
            END IF
            IF node.right != null THEN
                queue.enqueue(node.right)
            END IF
        END FOR
        result.append(currentLevel)
    END WHILE
    RETURN result
END FUNCTION
Complexity:
Time: O(n)

Space: O(w) where w = max width of tree

6. Morris Inorder Traversal (O(1) Space)
Use Case:
Space-constrained environments (embedded systems).

Deep trees where stack overflow is risk.

Interview optimization question.

Implementation
text
FUNCTION morrisInorder(root)
    result = []
    curr = root
    WHILE curr != null DO
        IF curr.left == null THEN
            result.append(curr.value)
            curr = curr.right
        ELSE
            predecessor = curr.left
            WHILE predecessor.right != null AND predecessor.right != curr DO
                predecessor = predecessor.right
            END WHILE
            IF predecessor.right == null THEN
                predecessor.right = curr
                curr = curr.left
            ELSE
                result.append(curr.value)
                predecessor.right = null
                curr = curr.right
            END IF
        END IF
    END WHILE
    RETURN result
END FUNCTION
Complexity:
Time: O(n)

Space: O(1) (only pointers, no extra data structures)

Note:
Temporarily modifies tree structure (back-links) but restores it.

7. Boundary Traversal
Use Case:
View tree from outside (boundary nodes only).

Partial tree representation.

Implementation
text
FUNCTION boundaryTraversal(root)
    result = []
    IF root == null THEN
        RETURN result
    END IF
    result.append(root.value)
    
    // Left boundary (excluding leaves)
    curr = root.left
    WHILE curr != null DO
        IF NOT isLeaf(curr) THEN
            result.append(curr.value)
        END IF
        curr = (curr.left != null) ? curr.left : curr.right
    END WHILE
    
    // All leaves
    FOR leaf IN getLeaves(root) DO
        result.append(leaf.value)
    END FOR
    
    // Right boundary (excluding leaves, in reverse)
    rightBoundary = []
    curr = root.right
    WHILE curr != null DO
        IF NOT isLeaf(curr) THEN
            rightBoundary.append(curr.value)
        END IF
        curr = (curr.right != null) ? curr.right : curr.left
    END WHILE
    
    FOR i = rightBoundary.length - 1 DOWN TO 0 DO
        result.append(rightBoundary[i])
    END FOR
    
    RETURN result
END FUNCTION
8. Vertical Traversal
Use Case:
Print tree vertically (grouped by column).

Visual tree representation.

Implementation (with Column Tracking)
text
FUNCTION verticalTraversal(root)
    columnMap = {}  // column -> list of values
    queue = [(root, 0)]  // (node, column)
    
    WHILE NOT queue.isEmpty() DO
        node, col = queue.dequeue()
        IF col NOT IN columnMap THEN
            columnMap[col] = []
        END IF
        columnMap[col].append(node.value)
        IF node.left != null THEN
            queue.enqueue((node.left, col-1))
        END IF
        IF node.right != null THEN
            queue.enqueue((node.right, col+1))
        END IF
    END WHILE
    
    result = []
    FOR col IN SORTED(columnMap.keys()) DO
        result.append(columnMap[col])
    END FOR
    RETURN result
END FUNCTION
9. Diagonal Traversal
Use Case:
Print diagonals of tree (top-left to bottom-right).

Implementation
text
FUNCTION diagonalTraversal(root)
    diagonalMap = {}  // diagonal -> list of values
    queue = [(root, 0)]  // (node, diagonal)
    
    WHILE NOT queue.isEmpty() DO
        node, diag = queue.dequeue()
        IF diag NOT IN diagonalMap THEN
            diagonalMap[diag] = []
        END IF
        diagonalMap[diag].append(node.value)
        IF node.left != null THEN
            queue.enqueue((node.left, diag+1))
        END IF
        IF node.right != null THEN
            queue.enqueue((node.right, diag))
        END IF
    END WHILE
    
    result = []
    FOR diag IN SORTED(diagonalMap.keys()) DO
        result.append(diagonalMap[diag])
    END FOR
    RETURN result
END FUNCTION
10. Spiral (Zigzag) Traversal
Use Case:
Print tree in spiral/zigzag pattern (alternating left-to-right, right-to-left).

Implementation
text
FUNCTION spiralTraversal(root)
    result = []
    IF root == null THEN
        RETURN result
    END IF
    queue = [root]
    leftToRight = true
    
    WHILE NOT queue.isEmpty() DO
        levelSize = queue.size()
        currentLevel = []
        
        FOR i = 1 TO levelSize DO
            node = queue.dequeue()
            currentLevel.append(node.value)
            IF node.left != null THEN
                queue.enqueue(node.left)
            END IF
            IF node.right != null THEN
                queue.enqueue(node.right)
            END IF
        END FOR
        
        IF NOT leftToRight THEN
            reverse(currentLevel)
        END IF
        result.append(currentLevel)
        leftToRight = NOT leftToRight
    END WHILE
    
    RETURN result
END FUNCTION
11. Comparison of Traversals
Traversal	Time	Space	Use Case
Inorder	O(n)	O(h)	BST sorted output
Preorder	O(n)	O(h)	Serialization, copying
Postorder	O(n)	O(h)	Tree deletion, postfix
Level Order	O(n)	O(w)	Level-by-level
Morris Inorder	O(n)	O(1)	Space-constrained
Boundary	O(n)	O(h)	Boundary view
Vertical	O(n)	O(w)	Vertical view
Diagonal	O(n)	O(w)	Diagonal view
Spiral	O(n)	O(w)	Spiral view
12. Choosing the Right Traversal
Need sorted order? → Inorder

Serializing? → Preorder

Deleting tree? → Postorder

Level-by-level? → Level Order

Space critical? → Morris Inorder

Special views? → Boundary, Vertical, Diagonal, Spiral

13. Interview Patterns
Google, Facebook: Iterative traversals (avoid recursion).

Amazon: Level Order for tree level queries.

Microsoft: Morris traversal (space optimization).

Apple: Serialization (preorder).

All FAANG: Multiple traversal techniques, recursive + iterative.

14. Common Pitfalls
Forgetting to handle null nodes.

Stack overflow with very deep trees (use iterative or Morris).

Incorrect pointer restoration in Morris (creating cycles).

Off-by-one in iterative approaches.

Master all traversal techniques—they unlock tree problem solving, system design interviews, and elegant algorithms across data structures!