🟢 Examples: Trees – Essential, Advanced, and FAANG-Level Mastery

1. Basic Binary Tree Construction
   text
   CLASS TreeNode
   value
   left
   right

// Create a simple tree
1
/ \
 2 3
/ \
 4 5

root = TreeNode(1)
root.left = TreeNode(2)
root.right = TreeNode(3)
root.left.left = TreeNode(4)
root.left.right = TreeNode(5) 2. Inorder Traversal (BST Sorted Output)
text
FUNCTION inorderTraversal(node, result)
IF node == null THEN
RETURN
END IF
inorderTraversal(node.left, result)
result.append(node.value)
inorderTraversal(node.right, result)
END FUNCTION
Output for BST: (sorted order)
Used in: SQL queries, sorted iteration.

3. Preorder Traversal (Serialize Tree)
   text
   FUNCTION preorderTraversal(node, result)
   IF node == null THEN
   RETURN
   END IF
   result.append(node.value)
   preorderTraversal(node.left, result)
   preorderTraversal(node.right, result)
   END FUNCTION
   Output:
   Used in: Serialize/copy tree, expression evaluation.

4. Postorder Traversal (Delete Tree)
   text
   FUNCTION postorderTraversal(node, result)
   IF node == null THEN
   RETURN
   END IF
   postorderTraversal(node.left, result)
   postorderTraversal(node.right, result)
   result.append(node.value)
   END FUNCTION
   Output:
   Used in: Delete tree, postfix notation.

5. Level Order Traversal (BFS with Queue)
   text
   FUNCTION levelOrderTraversal(root)
   queue = Queue()
   queue.enqueue(root)
   result = []
   WHILE NOT queue.isEmpty() DO
   node = queue.dequeue()
   result.append(node.value)
   IF node.left != null THEN
   queue.enqueue(node.left)
   END IF
   IF node.right != null THEN
   queue.enqueue(node.right)
   END IF
   END WHILE
   RETURN result
   END FUNCTION
   Output:
   Interviewed at: Google, Amazon, Facebook.

6. Binary Search Tree – Insert
   text
   FUNCTION insertBST(node, value)
   IF node == null THEN
   RETURN TreeNode(value)
   END IF
   IF value < node.value THEN
   node.left = insertBST(node.left, value)
   ELSE IF value > node.value THEN
   node.right = insertBST(node.right, value)
   END IF
   RETURN node
   END FUNCTION
   Complexity: O(log n) average, O(n) worst (skewed).

7. BST – Search
   text
   FUNCTION searchBST(node, target)
   IF node == null THEN
   RETURN false
   END IF
   IF target == node.value THEN
   RETURN true
   ELSE IF target < node.value THEN
   RETURN searchBST(node.left, target)
   ELSE
   RETURN searchBST(node.right, target)
   END IF
   END FUNCTION
8. Lowest Common Ancestor (LCA) – FAANG Classic
   text
   FUNCTION lowestCommonAncestor(node, p, q)
   IF node == null THEN
   RETURN null
   END IF
   IF node.value > max(p.value, q.value) THEN
   RETURN lowestCommonAncestor(node.left, p, q)
   ELSE IF node.value < min(p.value, q.value) THEN
   RETURN lowestCommonAncestor(node.right, p, q)
   ELSE
   RETURN node
   END IF
   END FUNCTION
   Interviewed at: Google, Facebook, Amazon.

9. Maximum Path Sum (Complex, Hard)
   text
   CLASS Helper
   maxSum = INT_MIN

FUNCTION maxPathSumHelper(node)
IF node == null THEN
RETURN 0
END IF
leftSum = MAX(0, maxPathSumHelper(node.left))
rightSum = MAX(0, maxPathSumHelper(node.right))
currentMax = node.value + leftSum + rightSum
maxSum = MAX(maxSum, currentMax)
RETURN node.value + MAX(leftSum, rightSum)
END FUNCTION
Interviewed at: Google, Apple, Microsoft.

10. Serialize and Deserialize Binary Tree
    text
    FUNCTION serialize(root)
    result = []
    FUNCTION helper(node)
    IF node == null THEN
    result.append("null")
    RETURN
    END IF
    result.append(node.value)
    helper(node.left)
    helper(node.right)
    END FUNCTION
    helper(root)
    RETURN ",".join(result)

FUNCTION deserialize(data)
nodes = data.split(",")
idx = 0
FUNCTION helper()
IF nodes[idx] == "null" THEN
idx += 1
RETURN null
END IF
node = TreeNode(INT(nodes[idx]))
idx += 1
node.left = helper()
node.right = helper()
RETURN node
END FUNCTION
RETURN helper()
END FUNCTION
Interviewed at: Uber, Google, Facebook.

11. Binary Tree Diameter (Longest Path)
    text
    CLASS Helper
    diameter = 0

FUNCTION diameterHelper(node)
IF node == null THEN
RETURN 0
END IF
leftHeight = diameterHelper(node.left)
rightHeight = diameterHelper(node.right)
diameter = MAX(diameter, leftHeight + rightHeight)
RETURN 1 + MAX(leftHeight, rightHeight)
END FUNCTION
Interviewed at: Amazon, Google.

12. Validate Binary Search Tree (Common)
    text
    FUNCTION isValidBST(node, minVal = INT_MIN, maxVal = INT_MAX)
    IF node == null THEN
    RETURN true
    END IF
    IF node.value <= minVal OR node.value >= maxVal THEN
    RETURN false
    END IF
    RETURN isValidBST(node.left, minVal, node.value) AND
    isValidBST(node.right, node.value, maxVal)
    END FUNCTION
    Interviewed at: All FAANG companies.

13. Balanced Binary Tree Check
    text
    FUNCTION isBalanced(node)
    FUNCTION helper(node)
    IF node == null THEN
    RETURN (true, 0) // (isBalanced, height)
    END IF
    leftBalanced, leftHeight = helper(node.left)
    rightBalanced, rightHeight = helper(node.right)
    balanced = leftBalanced AND rightBalanced AND
    ABS(leftHeight - rightHeight) <= 1
    RETURN (balanced, 1 + MAX(leftHeight, rightHeight))
    END FUNCTION
    RETURN helper(node)[0]
    END FUNCTION
14. Tree Path Sum
    text
    FUNCTION pathSum(root, targetSum)
    FUNCTION helper(node, remaining)
    IF node == null THEN
    RETURN 0
    END IF
    remaining -= node.value
    count = 0
    IF remaining == 0 THEN
    count = 1
    END IF
    count += helper(node.left, remaining) + helper(node.right, remaining)
    RETURN count
    END FUNCTION
    RETURN helper(root, targetSum)
    END FUNCTION
    Interviewed at: Facebook, Amazon.

15. Trie (Prefix Tree) – Autocomplete
    text
    CLASS TrieNode
    children = {}
    isEndOfWord = false

CLASS Trie
root = TrieNode()

    FUNCTION insert(word)
        node = root
        FOR char IN word DO
            IF char NOT IN node.children THEN
                node.children[char] = TrieNode()
            END IF
            node = node.children[char]
        END FOR
        node.isEndOfWord = true

    FUNCTION search(word)
        node = root
        FOR char IN word DO
            IF char NOT IN node.children THEN
                RETURN false
            END IF
            node = node.children[char]
        END FOR
        RETURN node.isEndOfWord

    FUNCTION startsWith(prefix)
        node = root
        FOR char IN prefix DO
            IF char NOT IN node.children THEN
                RETURN false
            END IF
            node = node.children[char]
        END FOR
        RETURN true

END CLASS
Used in: Google search suggestions, IDE autocomplete.

16. Min Heap (Priority Queue Implementation)
    text
    CLASS MinHeap
    arr = []

        FUNCTION push(val)
            arr.append(val)
            heapifyUp(arr.length - 1)

        FUNCTION heapifyUp(idx)
            WHILE idx > 0 AND arr[(idx-1)//2] > arr[idx] DO
                SWAP(arr[idx], arr[(idx-1)//2])
                idx = (idx-1)//2
            END WHILE

        FUNCTION pop()
            IF arr.length == 0 THEN
                ERROR "Empty heap"
            END IF
            root = arr[0]
            arr[0] = arr[arr.length-1]
            arr.pop()
            heapifyDown(0)
            RETURN root

        FUNCTION heapifyDown(idx)
            WHILE 2*idx + 1 < arr.length DO
                minIdx = idx
                IF arr[2*idx+1] < arr[minIdx] THEN
                    minIdx = 2*idx+1
                END IF
                IF 2*idx+2 < arr.length AND arr[2*idx+2] < arr[minIdx] THEN
                    minIdx = 2*idx+2
                END IF
                IF minIdx == idx THEN
                    BREAK
                END IF
                SWAP(arr[idx], arr[minIdx])
                idx = minIdx
            END WHILE

    END CLASS

17. Real-World: File System Hierarchy (N-ary Tree)
    text
    CLASS FileNode
    name
    isFile
    children = []

// Traverse and find all files
FUNCTION findAllFiles(node, allFiles)
IF node.isFile THEN
allFiles.append(node.name)
ELSE
FOR child IN node.children DO
findAllFiles(child, allFiles)
END FOR
END IF
END FUNCTION 18. Morris Inorder Traversal (O(1) Space)
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
Complexity: O(n) time, O(1) space.

These examples cover everything from interview classics to production-grade patterns—master them to own trees at any level!
