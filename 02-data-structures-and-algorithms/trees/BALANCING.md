🟢 Tree Balancing: Techniques, Self-Balancing Trees & Mastery
1. Why Balancing Matters
The Problem: Skewed Trees
Unsorted insertions into BST can create a linked-list-like structure.

Example: Insert 1, 2, 3, 4, 5 → degenerate tree with O(n) search.

The Solution: Self-Balancing
Maintain balance via rotations after insertions/deletions.

Guarantee O(log n) operations: search, insert, delete.

2. Balance Concepts
Height of Node
text
height(node) = 1 + max(height(node.left), height(node.right))
height(null) = 0
Balance Factor (AVL Trees)
text
balanceFactor(node) = height(node.left) - height(node.right)
Valid range: -1, 0, 1 (balanced)

Outside range: rebalancing required

Depth vs Height
Depth: Distance from root to node

Height: Distance from node to farthest leaf

3. Tree Rotations
Rotations are the core mechanism for rebalancing without changing BST property.

Left Rotation (clockwise)
text
FUNCTION rotateLeft(x)
    y = x.right
    T2 = y.left
    
    // Perform rotation
    y.left = x
    x.right = T2
    
    // Update heights
    x.height = 1 + max(height(x.left), height(x.right))
    y.height = 1 + max(height(y.left), height(y.right))
    
    RETURN y
END FUNCTION
Visual:

text
    x                y
     \              / \
      y      =>    x   c
     / \            \
    b   c            b
Right Rotation (counter-clockwise)
text
FUNCTION rotateRight(y)
    x = y.left
    T2 = x.right
    
    // Perform rotation
    x.right = y
    y.left = T2
    
    // Update heights
    y.height = 1 + max(height(y.left), height(y.right))
    x.height = 1 + max(height(x.left), height(x.right))
    
    RETURN x
END FUNCTION
Visual:

text
      y            x
     /            / \
    x     =>     a   y
   / \              /
  a   b            b
Left-Right Rotation (LR)
text
FUNCTION rotateLeftRight(node)
    node.left = rotateLeft(node.left)
    RETURN rotateRight(node)
END FUNCTION
Visual:

text
    z              z              x
   /              /              / \
  x      =>      y      =>      y   z
   \            /
    y          x
Right-Left Rotation (RL)
text
FUNCTION rotateRightLeft(node)
    node.right = rotateRight(node.right)
    RETURN rotateLeft(node)
END FUNCTION
Visual:

text
  z                z              y
   \                \            / \
    x      =>        y    =>    z   x
   /                  \
  y                    x
4. AVL Trees (Self-Balancing BST)
Definition
Every node has balance factor in {-1, 0, 1}.

Rebalance after every insertion/deletion.

AVL Insert
text
FUNCTION insertAVL(node, value)
    IF node == null THEN
        RETURN TreeNode(value)
    END IF
    
    IF value < node.value THEN
        node.left = insertAVL(node.left, value)
    ELSE IF value > node.value THEN
        node.right = insertAVL(node.right, value)
    ELSE
        RETURN node  // Duplicate
    END IF
    
    // Update height
    node.height = 1 + max(height(node.left), height(node.right))
    
    // Get balance factor
    balance = getBalance(node)
    
    // Left-Left case
    IF balance > 1 AND value < node.left.value THEN
        RETURN rotateRight(node)
    END IF
    
    // Right-Right case
    IF balance < -1 AND value > node.right.value THEN
        RETURN rotateLeft(node)
    END IF
    
    // Left-Right case
    IF balance > 1 AND value > node.left.value THEN
        node.left = rotateLeft(node.left)
        RETURN rotateRight(node)
    END IF
    
    // Right-Left case
    IF balance < -1 AND value < node.right.value THEN
        node.right = rotateRight(node.right)
        RETURN rotateLeft(node)
    END IF
    
    RETURN node
END FUNCTION

FUNCTION getBalance(node)
    IF node == null THEN
        RETURN 0
    END IF
    RETURN height(node.left) - height(node.right)
END FUNCTION
AVL Delete
text
FUNCTION deleteAVL(node, value)
    IF node == null THEN
        RETURN null
    END IF
    
    IF value < node.value THEN
        node.left = deleteAVL(node.left, value)
    ELSE IF value > node.value THEN
        node.right = deleteAVL(node.right, value)
    ELSE
        // Node found
        IF node.left == null THEN
            RETURN node.right
        ELSE IF node.right == null THEN
            RETURN node.left
        ELSE
            // Two children: find inorder successor
            minNode = findMin(node.right)
            node.value = minNode.value
            node.right = deleteAVL(node.right, minNode.value)
        END IF
    END IF
    
    IF node == null THEN
        RETURN null
    END IF
    
    // Update height and rebalance
    node.height = 1 + max(height(node.left), height(node.right))
    balance = getBalance(node)
    
    // Apply rotations as in insert
    IF balance > 1 AND getBalance(node.left) >= 0 THEN
        RETURN rotateRight(node)
    END IF
    
    IF balance > 1 AND getBalance(node.left) < 0 THEN
        node.left = rotateLeft(node.left)
        RETURN rotateRight(node)
    END IF
    
    IF balance < -1 AND getBalance(node.right) <= 0 THEN
        RETURN rotateLeft(node)
    END IF
    
    IF balance < -1 AND getBalance(node.right) > 0 THEN
        node.right = rotateRight(node.right)
        RETURN rotateLeft(node)
    END IF
    
    RETURN node
END FUNCTION
Complexity:
Insert/Delete/Search: O(log n) guaranteed

Rotations: O(1) per operation

Space: O(n) for tree nodes

5. Red-Black Trees
Properties
Every node is Red or Black.

Root is Black.

Leaves (null) are Black.

Red node has Black children.

All paths from root to leaves have same number of Black nodes.

Advantages Over AVL
Fewer rotations during insert/delete.

Slightly less strict balance (allows height 2*log n vs AVL's log n).

Better performance in practice for most workloads.

Red-Black Insert
text
FUNCTION insertRedBlack(root, value)
    node = TreeNode(value)
    node.color = RED
    
    // Standard BST insert
    currNode = root
    parent = null
    WHILE currNode != null DO
        parent = currNode
        IF value < currNode.value THEN
            currNode = currNode.left
        ELSE
            currNode = currNode.right
        END IF
    END WHILE
    
    // Link new node
    IF value < parent.value THEN
        parent.left = node
    ELSE
        parent.right = node
    END IF
    
    node.parent = parent
    
    // Fix violations
    fixInsertViolations(node)
    
    // Ensure root is Black
    root.color = BLACK
END FUNCTION

FUNCTION fixInsertViolations(node)
    WHILE node.parent != null AND node.parent.color == RED DO
        IF node.parent == node.parent.parent.left THEN
            uncle = node.parent.parent.right
            IF uncle != null AND uncle.color == RED THEN
                // Case 1: Uncle is Red
                node.parent.color = BLACK
                uncle.color = BLACK
                node.parent.parent.color = RED
                node = node.parent.parent
            ELSE
                // Case 2/3: Uncle is Black
                IF node == node.parent.right THEN
                    // Left-Right case
                    node = node.parent
                    rotateLeft(node)
                END IF
                // Left-Left case
                node.parent.color = BLACK
                node.parent.parent.color = RED
                rotateRight(node.parent.parent)
            END IF
        ELSE
            // Mirror of above (parent is right child)
            // ...
        END IF
    END WHILE
END FUNCTION
Complexity:
Insert/Delete/Search: O(log n) guaranteed

Fewer rotations than AVL

Space: O(n)

Used in:
Java TreeMap, TreeSet

C++ std::map, std::set

Linux kernel (kernel data structures)

6. Splay Trees
Concept
Recently accessed node is moved to root via splaying.

No explicit balance factor; amortized O(log n).

Splaying Operations
text
FUNCTION splay(node)
    WHILE node != root DO
        IF node.parent == root THEN
            // Zig: node is child of root
            IF node == node.parent.left THEN
                rotateRight(node.parent)
            ELSE
                rotateLeft(node.parent)
            END IF
        ELSE IF node == node.parent.left AND 
                node.parent == node.parent.parent.left THEN
            // Zig-Zig (left-left)
            rotateRight(node.parent.parent)
            rotateRight(node.parent)
        ELSE IF node == node.parent.right AND 
                node.parent == node.parent.parent.right THEN
            // Zig-Zig (right-right)
            rotateLeft(node.parent.parent)
            rotateLeft(node.parent)
        ELSE
            // Zig-Zag
            IF node == node.parent.left THEN
                rotateRight(node.parent)
            ELSE
                rotateLeft(node.parent)
            END IF
            rotateRight(node.parent.parent) or rotateLeft(...)
        END IF
    END WHILE
END FUNCTION
Advantages:
Automatic load balancing (frequently accessed near root).

Temporal locality benefit.

Used in:
Cache systems

Competitive programming

7. B-Trees
Definition
Multi-way self-balancing tree.

Each node holds m keys and m+1 children.

All leaves at same depth.

Properties
Minimized disk I/O (many keys per node).

O(log n) search, insert, delete.

Balanced by design (no separate rebalancing).

Used in:
Databases (MySQL B+ trees, MongoDB)

File systems (NTFS, ext4)

8. Treaps (Randomized BSTs)
Concept
BST by value, min-heap by random priority.

Expected balance via randomization.

Benefits
Simple implementation

O(log n) expected time

No explicit rotations needed for balance

Used in:
Competitive programming

Memory-limited systems

9. Comparison of Balancing Techniques
Technique	Worst Insert	Worst Delete	Rotations	Complexity	Used In
AVL	O(log n)	O(log n)	O(1) per op	O(log n)	Precise apps
Red-Black	O(log n)	O(log n)	O(1) amortized	O(log n)	Java, C++
Splay	O(log n) amortized	O(log n) amortized	O(1) amortized	O(log n)	Caches
B-Tree	O(log n)	O(log n)	Implicit	O(log n)	Databases
Treap	O(log n) expected	O(log n) expected	O(log n) expected	O(log n)	CP
10. When to Use Each
AVL: Precise, lookup-heavy, need guaranteed balance.

Red-Black: General-purpose, fewer rotations, widely used (Java, C++).

Splay: Temporal locality matters (caches, recent access).

B-Tree: Disk-based, range queries, database indexing.

Treap: Simplicity, randomization acceptable.

11. Interview Patterns
AVL/Red-Black Insert/Delete: Google, Microsoft, Apple

Rotation Logic: Amazon, Facebook

Balance Factor Calculation: All FAANG

Red-Black Properties: LinkedIn, Oracle

Why Balance?: Behavioral question at top companies

12. Implementation Best Practices
Always update height/depth after rotations.

Carefully handle parent pointers.

Test edge cases: single node, two nodes, deep trees.

Verify balance factor/color properties post-operation.

Use iterative or recursion with careful stack management.

13. Common Pitfalls
Forgetting to update parent pointers.

Incorrect balance factor calculation.

Missing rotations after deletion.

Rotation sequence error (LR vs RL).

Not handling null children in rotations.

Master tree balancing to guarantee O(log n) performance—the foundation of scalable systems and FAANG excellence!