🟢 Tree Pitfalls: Common Bugs, Edge Cases & Defensive Coding
1. Null Pointer Dereference
Description:
Accessing .left, .right, or .value of a null node without checking.

Impact:
Runtime exceptions, program crashes.

Prevention:
Always check if node == null before dereferencing; use helper functions for safe access.

2. Stack Overflow from Deep Recursion
Description:
Recursively traversing a very deep or skewed tree exhausts the call stack.

Impact:
Stack overflow error, crash, inability to handle large trees.

Prevention:
Use iterative traversal with explicit stack for deep trees (>1000 depth); consider Morris traversal (O(1) space).

text
// Unoptimized: Recursion on deep tree
FUNCTION inorder(node)
    inorder(node.left)      // Stack grows for each recursion
    process(node)
    inorder(node.right)
END FUNCTION

// Optimized: Iterative or Morris
FUNCTION iterativeInorder(root)
    // Explicit stack, bounded by actual tree height
END FUNCTION
3. Losing Root Pointer After Modifications
Description:
Rebalancing operations (rotations) can change the root; if not tracked, tree becomes unreachable.

Impact:
Memory leak, entire tree lost.

Prevention:
Always return updated root from insert/delete; maintain root reference properly.

text
// Unoptimized
tree.root = insert(tree.root, value)  // May change if rotation happens

// Safe
FUNCTION insert(root, value)
    // ... rotations may happen ...
    RETURN newRoot  // Updated root returned
END FUNCTION
4. Incorrect BST Property Validation
Description:
Not checking bounds correctly; allowing node.value to equal parent value or exceed range.

Impact:
Broken BST property, failed searches, incorrect operations.

Prevention:
Use strict bounds: left < parent < right (not <=).

text
// Unoptimized: Allows equality
IF node.value <= node.parent.value THEN
    // Wrong: duplicate values break BST

// Correct: Strict bounds
FUNCTION isValidBST(node, minVal, maxVal)
    IF node.value <= minVal OR node.value >= maxVal THEN
        RETURN false  // Strict inequality
    END IF
END FUNCTION
5. Rotation Pointer Errors
Description:
Forgetting to update parent pointers, losing subtrees, or creating cycles during rotation.

Impact:
Broken tree structure, lost nodes, infinite loops.

Prevention:
Carefully track all 4 pointers in rotation: left child, right child, parent, and subtree links.

text
// Unoptimized: Missing parent update
FUNCTION rotateLeft(x)
    y = x.right
    x.right = y.left    // OK
    y.left = x          // OK
    // Missing: y.parent = x.parent, update grandparent link
END FUNCTION

// Correct: Update all links
FUNCTION rotateLeft(x)
    y = x.right
    x.right = y.left
    IF y.left != null THEN
        y.left.parent = x
    END IF
    y.left = x
    y.parent = x.parent
    IF x.parent == null THEN
        root = y
    ELSE IF x == x.parent.left THEN
        x.parent.left = y
    ELSE
        x.parent.right = y
    END IF
    x.parent = y
END FUNCTION
6. Incorrect Height/Balance Factor Calculation
Description:
Not updating heights after insertion/deletion, or calculating balance factor wrong.

Impact:
Rebalancing doesn't trigger, tree becomes unbalanced, degrading to O(n).

Prevention:
Update height bottom-up; recalculate after every structural change.

text
// Unoptimized: Forget to update height
FUNCTION insertAVL(node, value)
    // ... insert ...
    // Missing: node.height = 1 + max(height(left), height(right))
    RETURN node
END FUNCTION

// Correct: Always update
FUNCTION insertAVL(node, value)
    // ... insert ...
    node.height = 1 + max(height(node.left), height(node.right))
    balance = getBalance(node)
    // ... rebalance if needed ...
    RETURN node
END FUNCTION
7. Edge Cases: Empty, Single, or Two-Node Trees
Description:
Failing to handle boundary cases (no nodes, one node, two nodes).

Impact:
Test failures despite correct general logic.

Prevention:
Always test and explicitly handle: null tree, single node, two nodes.

text
// Always test these:
tree = null → operations fail gracefully
tree = [1] → single node tree
tree = [1, 2] → two-node tree
tree with duplicates → handle as needed
8. Traversal After Modification
Description:
Iterating and simultaneously modifying tree (insert/delete), causing skipped or revisited nodes.

Impact:
Incomplete traversal, infinite loops, data loss.

Prevention:
Don't modify while iterating; collect indices/nodes first, then modify.

text
// Unoptimized: Modify during iteration
FUNCTION deleteAll(node, value)
    IF node == null THEN RETURN null END IF
    IF node.value == value THEN
        // Delete might break traversal
        RETURN deleteNode(node)
    END IF
    node.left = deleteAll(node.left, value)
    node.right = deleteAll(node.right, value)
    RETURN node
END FUNCTION
9. Incorrect Serialization/Deserialization
Description:
Not including null markers, wrong parsing order, losing tree structure.

Impact:
Reconstructed tree is malformed, wrong structure, missing nodes.

Prevention:
Use preorder (root first) or level-order; include null markers explicitly.

text
// Unoptimized: No null markers
serialize: [1, 2, 3]  // Ambiguous reconstruction

// Correct: Include nulls
serialize: [1, 2, null, null, 3]  // Clear structure
10. Parent Pointer Inconsistency
Description:
Parent pointers not updated or become stale after rotations/deletions.

Impact:
Tree traversal breaks, parent queries return wrong node.

Prevention:
After any structural change, verify parent pointers; use parent-safe rotations.

11. Cycles Created in Tree
Description:
Incorrect link assignment creates a cycle (node → ancestor → node).

Impact:
Infinite loops in traversal, memory leak.

Prevention:
Never link child to ancestor; validate acyclic property post-operation.

text
// BUG: Creates cycle
node3.left = node1  // If node1 is ancestor of node3
12. Height Calculation Off-by-One
Description:
height(null) = -1 vs 0, or height calculation excludes node itself.

Impact:
Balance calculations wrong, incorrect rebalancing.

Prevention:
Define clearly: height(null) = 0 or -1 consistently; height(leaf) = 0 or 1.

text
// Common mistake: Inconsistent height definition
height(null) = 0  // vs -1 elsewhere → bugs

// Correct: Consistent definition
height(null) = 0
height(leaf) = 1
height(node) = 1 + max(height(left), height(right))
13. Over-Engineering: Tree Where Array Is Better
Description:
Using trees (especially complex ones like AVL) when array, list, or hash table is simpler/faster.

Impact:
Unnecessary complexity, hidden performance issues.

Prevention:
Analyze use case; don't default to trees. Consider: array (sorted), hash table (lookup), heap (priority).

14. Forgetting to Clear/Nullify Deleted Nodes
Description:
Removing node from tree but not releasing reference or clearing pointers.

Impact:
Memory leak (in C/C++), GC churn (Java/Python), stale references.

Prevention:
In languages without GC, explicitly free memory; nullify pointers.

15. Incorrect LCA (Lowest Common Ancestor) Logic
Description:
Not checking if both nodes exist in tree, or returning wrong ancestor.

Impact:
Wrong LCA, failed interview question.

Prevention:
Verify both nodes are in tree; handle case where one/both missing.

text
// Unoptimized: Assumes nodes in tree
FUNCTION lca(node, p, q)
    IF node == null THEN RETURN null END IF
    // ... assumes p, q are in tree

// Safe: Verify existence
FUNCTION lcaSafe(node, p, q)
    IF NOT (exists(node, p) AND exists(node, q)) THEN
        RETURN null  // One or both missing
    END IF
    RETURN lca(node, p, q)
END FUNCTION
16. Forgetting Parent Update in Deletions
Description:
After deleting node, parent's pointer still references it instead of replacement.

Impact:
Tree has unreachable/duplicate nodes.

Prevention:
After delete, verify parent.left/right updated to new node.

17. Race Conditions in Concurrent Trees
Description:
Multiple threads accessing/modifying tree without synchronization.

Impact:
Data corruption, lost updates, crashes.

Prevention:
Use locks or thread-safe tree implementations; avoid concurrent modification without coordination.

18. Mishandling Duplicate Values
Description:
BST doesn't specify how to handle duplicates; inconsistent implementation.

Impact:
Search/delete behaves unexpectedly, lost nodes.

Prevention:
Decide upfront: duplicates allowed? If yes, how stored (left/right/count)?

19. Incorrect Rotation Sequence (LR vs RL)
Description:
Applying wrong rotation type for imbalance (left-right vs right-left).

Impact:
Tree remains unbalanced, rotations ineffective.

Prevention:
Check balance factor and child balance factor; apply correct sequence.

text
// Right case: balance < -1
IF getBalance(node.right) > 0 THEN
    // Right-Left case: rotate right child left, then node left
    node.right = rotateRight(node.right)
    RETURN rotateLeft(node)
ELSE
    // Right-Right case: rotate node left
    RETURN rotateLeft(node)
END IF
20. Assuming Tree Is Balanced Without Verification
Description:
Relying on O(log n) complexity without ensuring tree maintains balance.

Impact:
Performance degrades to O(n) in worst case.

Prevention:
Use proven self-balancing implementations (AVL, Red-Black); don't manually manage balance.

Master these pitfalls to write robust, production-grade tree code—avoiding bugs that waste days of debugging and tanking system performance!