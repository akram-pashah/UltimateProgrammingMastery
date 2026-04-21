🟢 Pseudocode: Trees – Core Operations to Advanced Algorithms

1. Basic Tree Node Structure
   text
   CLASS TreeNode
   value
   left
   right
   END CLASS

CLASS TrieNode
children = {} // Dictionary mapping char to TrieNode
isEndOfWord = false
END CLASS

CLASS HeapNode
value
// For implicit heap: children at 2*i+1 and 2*i+2
END CLASS 2. Inorder Traversal (Recursive)
text
FUNCTION inorderTraversal(node, result)
IF node == null THEN
RETURN
END IF
inorderTraversal(node.left, result)
result.append(node.value)
inorderTraversal(node.right, result)
END FUNCTION 3. Inorder Traversal (Iterative with Stack)
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
END FUNCTION 4. Preorder Traversal
text
FUNCTION preorderTraversal(node, result)
IF node == null THEN
RETURN
END IF
result.append(node.value)
preorderTraversal(node.left, result)
preorderTraversal(node.right, result)
END FUNCTION 5. Postorder Traversal
text
FUNCTION postorderTraversal(node, result)
IF node == null THEN
RETURN
END IF
postorderTraversal(node.left, result)
postorderTraversal(node.right, result)
result.append(node.value)
END FUNCTION 6. Level Order Traversal (BFS)
text
FUNCTION levelOrderTraversal(root)
queue = Queue()
result = []
queue.enqueue(root)
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
END FUNCTION 7. Morris Inorder Traversal (O(1) Space)
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
END FUNCTION 8. BST Insert
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
END FUNCTION 9. BST Search
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
END FUNCTION 10. BST Delete
text
FUNCTION deleteBST(node, value)
IF node == null THEN
RETURN null
END IF
IF value < node.value THEN
node.left = deleteBST(node.left, value)
ELSE IF value > node.value THEN
node.right = deleteBST(node.right, value)
ELSE
// Node to delete found
IF node.left == null THEN
RETURN node.right
ELSE IF node.right == null THEN
RETURN node.left
ELSE
// Two children: find inorder successor
minNode = findMin(node.right)
node.value = minNode.value
node.right = deleteBST(node.right, minNode.value)
END IF
END IF
RETURN node
END FUNCTION

FUNCTION findMin(node)
WHILE node.left != null DO
node = node.left
END WHILE
RETURN node
END FUNCTION 11. Validate Binary Search Tree
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
END FUNCTION 12. Lowest Common Ancestor (LCA)
text
FUNCTION lowestCommonAncestor(node, p, q)
IF node == null THEN
RETURN null
END IF
IF node == p OR node == q THEN
RETURN node
END IF
left = lowestCommonAncestor(node.left, p, q)
right = lowestCommonAncestor(node.right, p, q)
IF left != null AND right != null THEN
RETURN node
END IF
RETURN left != null ? left : right
END FUNCTION 13. Maximum Path Sum
text
CLASS PathSum
maxSum = INT_MIN

FUNCTION maxPathSum(root)
helper(root)
RETURN maxSum

FUNCTION helper(node)
IF node == null THEN
RETURN 0
END IF
leftSum = MAX(0, helper(node.left))
rightSum = MAX(0, helper(node.right))
currentMax = node.value + leftSum + rightSum
maxSum = MAX(maxSum, currentMax)
RETURN node.value + MAX(leftSum, rightSum)
END FUNCTION
END CLASS 14. Tree Diameter
text
CLASS TreeDiameter
diameter = 0

FUNCTION getDiameter(root)
helper(root)
RETURN diameter

FUNCTION helper(node)
IF node == null THEN
RETURN 0
END IF
leftHeight = helper(node.left)
rightHeight = helper(node.right)
diameter = MAX(diameter, leftHeight + rightHeight)
RETURN 1 + MAX(leftHeight, rightHeight)
END FUNCTION
END CLASS 15. Is Balanced Binary Tree
text
FUNCTION isBalanced(node)
FUNCTION helper(node)
IF node == null THEN
RETURN (true, 0)
END IF
leftBalanced, leftHeight = helper(node.left)
rightBalanced, rightHeight = helper(node.right)
balanced = leftBalanced AND rightBalanced AND
ABS(leftHeight - rightHeight) <= 1
RETURN (balanced, 1 + MAX(leftHeight, rightHeight))
END FUNCTION
RETURN helper(node)[0]
END FUNCTION 16. AVL Tree – Right Rotation
text
FUNCTION rotateRight(y)
x = y.left
T2 = x.right
x.right = y
y.left = T2
y.height = 1 + MAX(getHeight(y.left), getHeight(y.right))
x.height = 1 + MAX(getHeight(x.left), getHeight(x.right))
RETURN x
END FUNCTION 17. AVL Tree – Left Rotation
text
FUNCTION rotateLeft(x)
y = x.right
T2 = y.left
y.left = x
x.right = T2
x.height = 1 + MAX(getHeight(x.left), getHeight(x.right))
y.height = 1 + MAX(getHeight(y.left), getHeight(y.right))
RETURN y
END FUNCTION 18. AVL Tree – Insert
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
RETURN node
END IF
node.height = 1 + MAX(getHeight(node.left), getHeight(node.right))
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
RETURN getHeight(node.left) - getHeight(node.right)
END FUNCTION

FUNCTION getHeight(node)
IF node == null THEN
RETURN 0
END IF
RETURN node.height
END FUNCTION 19. Trie – Insert
text
FUNCTION trieInsert(root, word)
node = root
FOR char IN word DO
IF char NOT IN node.children THEN
node.children[char] = TrieNode()
END IF
node = node.children[char]
END FOR
node.isEndOfWord = true
END FUNCTION 20. Trie – Search
text
FUNCTION trieSearch(root, word)
node = root
FOR char IN word DO
IF char NOT IN node.children THEN
RETURN false
END IF
node = node.children[char]
END FOR
RETURN node.isEndOfWord
END FUNCTION 21. Trie – StartsWith (Prefix Match)
text
FUNCTION trieStartsWith(root, prefix)
node = root
FOR char IN prefix DO
IF char NOT IN node.children THEN
RETURN false
END IF
node = node.children[char]
END FOR
RETURN true
END FUNCTION 22. Min Heap – Push
text
FUNCTION heapPush(arr, val)
arr.append(val)
heapifyUp(arr, arr.length - 1)
END FUNCTION

FUNCTION heapifyUp(arr, idx)
WHILE idx > 0 AND arr[(idx-1)//2] > arr[idx] DO
SWAP(arr[idx], arr[(idx-1)//2])
idx = (idx-1)//2
END WHILE
END FUNCTION 23. Min Heap – Pop
text
FUNCTION heapPop(arr)
IF arr.length == 0 THEN
ERROR "Heap empty"
END IF
root = arr[0]
arr[0] = arr[arr.length-1]
arr.removeLast()
heapifyDown(arr, 0)
RETURN root
END FUNCTION

FUNCTION heapifyDown(arr, idx)
WHILE 2*idx + 1 < arr.length DO
minIdx = idx
IF arr[2*idx+1] < arr[minIdx] THEN
minIdx = 2*idx+1
END IF
IF 2*idx+2 < arr.length AND arr[2*idx+2] < arr[minIdx] THEN
minIdx = 2\*idx+2
END IF
IF minIdx == idx THEN
BREAK
END IF
SWAP(arr[idx], arr[minIdx])
idx = minIdx
END WHILE
END FUNCTION 24. Build Heap (Heapify)
text
FUNCTION buildHeap(arr)
FOR i = arr.length // 2 - 1 DOWN TO 0 DO
heapifyDown(arr, i)
END FOR
END FUNCTION 25. Segment Tree – Update
text
FUNCTION segmentTreeUpdate(node, start, end, idx, val)
IF start == end THEN
node.val = val
ELSE
mid = (start + end) // 2
IF idx <= mid THEN
segmentTreeUpdate(node.left, start, mid, idx, val)
ELSE
segmentTreeUpdate(node.right, mid+1, end, idx, val)
END IF
node.val = node.left.val + node.right.val
END IF
END FUNCTION 26. Segment Tree – Range Query
text
FUNCTION segmentTreeQuery(node, start, end, l, r)
IF r < start OR end < l THEN
RETURN 0
END IF
IF l <= start AND end <= r THEN
RETURN node.val
END IF
mid = (start + end) // 2
leftSum = segmentTreeQuery(node.left, start, mid, l, r)
rightSum = segmentTreeQuery(node.right, mid+1, end, l, r)
RETURN leftSum + rightSum
END FUNCTION 27. Fenwick Tree (Binary Indexed Tree) – Update
text
FUNCTION fenwickUpdate(tree, idx, delta)
WHILE idx < tree.length DO
tree[idx] += delta
idx += idx AND (-idx)
END WHILE
END FUNCTION 28. Fenwick Tree – Query
text
FUNCTION fenwickQuery(tree, idx)
sum = 0
WHILE idx > 0 DO
sum += tree[idx]
idx -= idx AND (-idx)
END WHILE
RETURN sum
END FUNCTION 29. Serialize and Deserialize Tree
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
END FUNCTION

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
Master these pseudocode implementations to solidify your understanding and excel in tree-based problem solving!
