# 🟢 Pseudocode: Sets & Maps – Core Operations to Advanced Algorithms

## 1. Basic Hash Function

```text
FUNCTION simpleHash(key, tableSize)
    // Sum of character ASCII values modulo table size
    hash = 0
    FOR char IN key DO
        hash += ASCII(char)
    END FOR
    RETURN hash % tableSize
END FUNCTION

FUNCTION polynomialRollingHash(key, tableSize)
    // Polynomial rolling hash: O(|key|)
    hash = 0
    base = 31
    power = 1
    FOR char IN key DO
        hash += (ASCII(char) * power) % tableSize
        power = (power * base) % tableSize
    END FOR
    RETURN hash % tableSize
END FUNCTION

FUNCTION djb2Hash(key, tableSize)
    // DJB2 hash function (good distribution)
    hash = 5381
    FOR char IN key DO
        hash = ((hash << 5) + hash) + ASCII(char)
    END FOR
    RETURN ABS(hash) % tableSize
END FUNCTION

FUNCTION fnvHash(key, tableSize)
    // FNV-1a hash function (excellent distribution)
    hash = 2166136261  // FNV offset basis
    FOR char IN key DO
        hash = hash XOR ASCII(char)
        hash = hash * 16777619  // FNV prime
    END FOR
    RETURN hash % tableSize
END FUNCTION

FUNCTION murmurHash(key, tableSize)
    // Murmur3 hash (32-bit variant)
    hash = 0
    c1 = 0xcc9e2d51
    c2 = 0x1b873593
    
    FOR each 4-byte chunk IN key DO
        k = chunk
        k = k * c1
        k = ROTL32(k, 15)
        k = k * c2
        
        hash = hash XOR k
        hash = ROTL32(hash, 13)
        hash = hash * 5 + 0xe6546b64
    END FOR
    
    // Handle remaining bytes
    // ... (tail processing)
    
    // Finalization
    hash = hash XOR len(key)
    hash = hash XOR (hash >> 16)
    hash = hash * 0x85ebca6b
    hash = hash XOR (hash >> 13)
    hash = hash * 0xc2b2ae35
    hash = hash XOR (hash >> 16)
    
    RETURN hash % tableSize
END FUNCTION
```

***

## 2. Hash Set Implementation

### a. Hash Set with Separate Chaining

```text
CLASS HashSet
    buckets = ARRAY[size]
    size = 16
    count = 0
    loadFactorThreshold = 0.75
    
    FUNCTION hash(element)
        hash = 0
        FOR char IN element.toString() DO
            hash = (hash * 31 + ASCII(char)) % LARGE_PRIME
        END FOR
        RETURN hash % size
    END FUNCTION
    
    FUNCTION add(element)
        idx = hash(element)
        IF buckets[idx] == null THEN
            buckets[idx] = []
        END IF
        
        // Check if element exists
        FOR item IN buckets[idx] DO
            IF item == element THEN
                RETURN false  // Already exists
            END IF
        END FOR
        
        // Add new element
        buckets[idx].append(element)
        count += 1
        
        // Check load factor; resize if needed
        IF count / size > loadFactorThreshold THEN
            resize()
        END IF
        
        RETURN true
    END FUNCTION
    
    FUNCTION remove(element)
        idx = hash(element)
        IF buckets[idx] == null THEN
            RETURN false
        END IF
        
        FOR i = 0 TO len(buckets[idx]) - 1 DO
            IF buckets[idx][i] == element THEN
                buckets[idx].removeAt(i)
                count -= 1
                RETURN true
            END IF
        END FOR
        
        RETURN false
    END FUNCTION
    
    FUNCTION contains(element)
        idx = hash(element)
        IF buckets[idx] == null THEN
            RETURN false
        END IF
        
        FOR item IN buckets[idx] DO
            IF item == element THEN
                RETURN true
            END IF
        END FOR
        
        RETURN false
    END FUNCTION
    
    FUNCTION size()
        RETURN count
    END FUNCTION
    
    FUNCTION isEmpty()
        RETURN count == 0
    END FUNCTION
    
    FUNCTION clear()
        buckets = ARRAY[size]
        count = 0
    END FUNCTION
    
    FUNCTION resize()
        oldBuckets = buckets
        oldSize = size
        
        size = oldSize * 2
        buckets = ARRAY[size]
        count = 0
        
        FOR bucket IN oldBuckets DO
            IF bucket != null THEN
                FOR element IN bucket DO
                    add(element)
                END FOR
            END IF
        END FOR
    END FUNCTION
END CLASS
```

**Complexity:** O(1) average add/remove/contains, O(n) worst-case.[1][2]

***

### b. Tree Set (BST-based)

```text
CLASS TreeSet
    root = null
    count = 0
    comparator = null
    
    CLASS Node
        value = null
        left = null
        right = null
        parent = null
        color = RED  // For Red-Black Tree
        
        FUNCTION __init__(value)
            THIS.value = value
        END FUNCTION
    END CLASS
    
    FUNCTION add(element)
        IF root == null THEN
            root = Node(element)
            root.color = BLACK
            count += 1
            RETURN true
        END IF
        
        node = root
        parent = null
        
        WHILE node != null DO
            parent = node
            cmp = compare(element, node.value)
            
            IF cmp < 0 THEN
                node = node.left
            ELSE IF cmp > 0 THEN
                node = node.right
            ELSE
                RETURN false  // Duplicate
            END IF
        END WHILE
        
        newNode = Node(element)
        newNode.parent = parent
        
        IF compare(element, parent.value) < 0 THEN
            parent.left = newNode
        ELSE
            parent.right = newNode
        END IF
        
        count += 1
        fixAfterInsertion(newNode)
        RETURN true
    END FUNCTION
    
    FUNCTION remove(element)
        node = findNode(element)
        IF node == null THEN
            RETURN false
        END IF
        
        deleteNode(node)
        count -= 1
        RETURN true
    END FUNCTION
    
    FUNCTION contains(element)
        RETURN findNode(element) != null
    END FUNCTION
    
    FUNCTION findNode(element)
        node = root
        WHILE node != null DO
            cmp = compare(element, node.value)
            IF cmp < 0 THEN
                node = node.left
            ELSE IF cmp > 0 THEN
                node = node.right
            ELSE
                RETURN node
            END IF
        END WHILE
        RETURN null
    END FUNCTION
    
    FUNCTION first()
        IF root == null THEN
            ERROR "Set is empty"
        END IF
        node = root
        WHILE node.left != null DO
            node = node.left
        END WHILE
        RETURN node.value
    END FUNCTION
    
    FUNCTION last()
        IF root == null THEN
            ERROR "Set is empty"
        END IF
        node = root
        WHILE node.right != null DO
            node = node.right
        END WHILE
        RETURN node.value
    END FUNCTION
    
    FUNCTION higher(element)
        // Return smallest element > element
        node = root
        result = null
        
        WHILE node != null DO
            IF compare(element, node.value) < 0 THEN
                result = node.value
                node = node.left
            ELSE
                node = node.right
            END IF
        END WHILE
        
        RETURN result
    END FUNCTION
    
    FUNCTION lower(element)
        // Return largest element < element
        node = root
        result = null
        
        WHILE node != null DO
            IF compare(element, node.value) > 0 THEN
                result = node.value
                node = node.right
            ELSE
                node = node.left
            END IF
        END WHILE
        
        RETURN result
    END FUNCTION
    
    FUNCTION subSet(fromElement, toElement)
        // Return elements in range [fromElement, toElement)
        result = []
        inOrderTraversal(root, fromElement, toElement, result)
        RETURN result
    END FUNCTION
    
    FUNCTION inOrderTraversal(node, start, end, result)
        IF node == null THEN
            RETURN
        END IF
        
        IF compare(node.value, start) > 0 THEN
            inOrderTraversal(node.left, start, end, result)
        END IF
        
        IF compare(node.value, start) >= 0 AND compare(node.value, end) < 0 THEN
            result.append(node.value)
        END IF
        
        IF compare(node.value, end) < 0 THEN
            inOrderTraversal(node.right, start, end, result)
        END IF
    END FUNCTION
    
    FUNCTION compare(a, b)
        IF comparator != null THEN
            RETURN comparator(a, b)
        ELSE
            IF a < b THEN RETURN -1
            ELSE IF a > b THEN RETURN 1
            ELSE RETURN 0
        END IF
    END FUNCTION
    
    // Red-Black Tree balancing
    FUNCTION fixAfterInsertion(node)
        node.color = RED
        
        WHILE node != null AND node != root AND node.parent.color == RED DO
            IF node.parent == node.parent.parent.left THEN
                uncle = node.parent.parent.right
                IF uncle != null AND uncle.color == RED THEN
                    node.parent.color = BLACK
                    uncle.color = BLACK
                    node.parent.parent.color = RED
                    node = node.parent.parent
                ELSE
                    IF node == node.parent.right THEN
                        node = node.parent
                        rotateLeft(node)
                    END IF
                    node.parent.color = BLACK
                    node.parent.parent.color = RED
                    rotateRight(node.parent.parent)
                END IF
            ELSE
                // Mirror case
                uncle = node.parent.parent.left
                IF uncle != null AND uncle.color == RED THEN
                    node.parent.color = BLACK
                    uncle.color = BLACK
                    node.parent.parent.color = RED
                    node = node.parent.parent
                ELSE
                    IF node == node.parent.left THEN
                        node = node.parent
                        rotateRight(node)
                    END IF
                    node.parent.color = BLACK
                    node.parent.parent.color = RED
                    rotateLeft(node.parent.parent)
                END IF
            END IF
        END WHILE
        
        root.color = BLACK
    END FUNCTION
    
    FUNCTION rotateLeft(node)
        right = node.right
        node.right = right.left
        
        IF right.left != null THEN
            right.left.parent = node
        END IF
        
        right.parent = node.parent
        
        IF node.parent == null THEN
            root = right
        ELSE IF node == node.parent.left THEN
            node.parent.left = right
        ELSE
            node.parent.right = right
        END IF
        
        right.left = node
        node.parent = right
    END FUNCTION
    
    FUNCTION rotateRight(node)
        left = node.left
        node.left = left.right
        
        IF left.right != null THEN
            left.right.parent = node
        END IF
        
        left.parent = node.parent
        
        IF node.parent == null THEN
            root = left
        ELSE IF node == node.parent.right THEN
            node.parent.right = left
        ELSE
            node.parent.left = left
        END IF
        
        left.right = node
        node.parent = left
    END FUNCTION
END CLASS
```

**Complexity:** O(log n) guaranteed for all operations.[2][3][1]

***

### c. Linked Hash Set (Insertion Order)

```text
CLASS LinkedHashSet
    map = HashMap()  // element -> Node
    head = Node(null)  // Dummy head
    tail = Node(null)  // Dummy tail
    count = 0
    
    CLASS Node
        element = null
        prev = null
        next = null
        
        FUNCTION __init__(element)
            THIS.element = element
        END FUNCTION
    END CLASS
    
    FUNCTION __init__()
        head.next = tail
        tail.prev = head
    END FUNCTION
    
    FUNCTION add(element)
        IF map.containsKey(element) THEN
            RETURN false
        END IF
        
        node = Node(element)
        map.put(element, node)
        addToTail(node)
        count += 1
        RETURN true
    END FUNCTION
    
    FUNCTION remove(element)
        IF NOT map.containsKey(element) THEN
            RETURN false
        END IF
        
        node = map.get(element)
        removeNode(node)
        map.remove(element)
        count -= 1
        RETURN true
    END FUNCTION
    
    FUNCTION contains(element)
        RETURN map.containsKey(element)
    END FUNCTION
    
    FUNCTION addToTail(node)
        node.prev = tail.prev
        node.next = tail
        tail.prev.next = node
        tail.prev = node
    END FUNCTION
    
    FUNCTION removeNode(node)
        node.prev.next = node.next
        node.next.prev = node.prev
    END FUNCTION
    
    FUNCTION iterator()
        current = head.next
        WHILE current != tail DO
            YIELD current.element
            current = current.next
        END WHILE
    END FUNCTION
END CLASS
```

**Complexity:** O(1) add/remove/contains, O(n) iteration in insertion order.[1]

***

## 3. Hash Map Implementation

### a. Hash Map with Separate Chaining

```text
CLASS HashMap
    buckets = ARRAY[size]
    size = 16
    count = 0
    loadFactorThreshold = 0.75
    
    CLASS Entry
        key = null
        value = null
        
        FUNCTION __init__(key, value)
            THIS.key = key
            THIS.value = value
        END FUNCTION
    END CLASS
    
    FUNCTION hash(key)
        hash = 0
        FOR char IN key.toString() DO
            hash = (hash * 31 + ASCII(char)) % LARGE_PRIME
        END FOR
        RETURN hash % size
    END FUNCTION
    
    FUNCTION put(key, value)
        idx = hash(key)
        IF buckets[idx] == null THEN
            buckets[idx] = []
        END IF
        
        // Update if key exists
        FOR entry IN buckets[idx] DO
            IF entry.key == key THEN
                oldValue = entry.value
                entry.value = value
                RETURN oldValue
            END IF
        END FOR
        
        // Add new entry
        buckets[idx].append(Entry(key, value))
        count += 1
        
        IF count / size > loadFactorThreshold THEN
            resize()
        END IF
        
        RETURN null
    END FUNCTION
    
    FUNCTION get(key)
        idx = hash(key)
        IF buckets[idx] == null THEN
            RETURN null
        END IF
        
        FOR entry IN buckets[idx] DO
            IF entry.key == key THEN
                RETURN entry.value
            END IF
        END FOR
        
        RETURN null
    END FUNCTION
    
    FUNCTION remove(key)
        idx = hash(key)
        IF buckets[idx] == null THEN
            RETURN null
        END IF
        
        FOR i = 0 TO len(buckets[idx]) - 1 DO
            IF buckets[idx][i].key == key THEN
                value = buckets[idx][i].value
                buckets[idx].removeAt(i)
                count -= 1
                RETURN value
            END IF
        END FOR
        
        RETURN null
    END FUNCTION
    
    FUNCTION containsKey(key)
        RETURN get(key) != null
    END FUNCTION
    
    FUNCTION containsValue(value)
        FOR bucket IN buckets DO
            IF bucket != null THEN
                FOR entry IN bucket DO
                    IF entry.value == value THEN
                        RETURN true
                    END IF
                END FOR
            END IF
        END FOR
        RETURN false
    END FUNCTION
    
    FUNCTION keys()
        keySet = SET()
        FOR bucket IN buckets DO
            IF bucket != null THEN
                FOR entry IN bucket DO
                    keySet.add(entry.key)
                END FOR
            END IF
        END FOR
        RETURN keySet
    END FUNCTION
    
    FUNCTION values()
        valueList = []
        FOR bucket IN buckets DO
            IF bucket != null THEN
                FOR entry IN bucket DO
                    valueList.append(entry.value)
                END FOR
            END IF
        END FOR
        RETURN valueList
    END FUNCTION
    
    FUNCTION entries()
        entrySet = SET()
        FOR bucket IN buckets DO
            IF bucket != null THEN
                FOR entry IN bucket DO
                    entrySet.add((entry.key, entry.value))
                END FOR
            END IF
        END FOR
        RETURN entrySet
    END FUNCTION
    
    FUNCTION size()
        RETURN count
    END FUNCTION
    
    FUNCTION isEmpty()
        RETURN count == 0
    END FUNCTION
    
    FUNCTION clear()
        buckets = ARRAY[size]
        count = 0
    END FUNCTION
    
    FUNCTION resize()
        oldBuckets = buckets
        oldSize = size
        
        size = oldSize * 2
        buckets = ARRAY[size]
        count = 0
        
        FOR bucket IN oldBuckets DO
            IF bucket != null THEN
                FOR entry IN bucket DO
                    put(entry.key, entry.value)
                END FOR
            END IF
        END FOR
    END FUNCTION
END CLASS
```

**Complexity:** O(1) average put/get/remove, O(n) containsValue.[4][1]

***

### b. Tree Map (BST-based)

```text
CLASS TreeMap
    root = null
    count = 0
    
    CLASS Node
        key = null
        value = null
        left = null
        right = null
        parent = null
        color = RED
        
        FUNCTION __init__(key, value)
            THIS.key = key
            THIS.value = value
        END FUNCTION
    END CLASS
    
    FUNCTION put(key, value)
        IF root == null THEN
            root = Node(key, value)
            root.color = BLACK
            count += 1
            RETURN null
        END IF
        
        node = root
        parent = null
        
        WHILE node != null DO
            parent = node
            cmp = compare(key, node.key)
            
            IF cmp < 0 THEN
                node = node.left
            ELSE IF cmp > 0 THEN
                node = node.right
            ELSE
                oldValue = node.value
                node.value = value
                RETURN oldValue
            END IF
        END WHILE
        
        newNode = Node(key, value)
        newNode.parent = parent
        
        IF compare(key, parent.key) < 0 THEN
            parent.left = newNode
        ELSE
            parent.right = newNode
        END IF
        
        count += 1
        fixAfterInsertion(newNode)
        RETURN null
    END FUNCTION
    
    FUNCTION get(key)
        node = findNode(key)
        RETURN node.value IF node != null ELSE null
    END FUNCTION
    
    FUNCTION remove(key)
        node = findNode(key)
        IF node == null THEN
            RETURN null
        END IF
        
        value = node.value
        deleteNode(node)
        count -= 1
        RETURN value
    END FUNCTION
    
    FUNCTION findNode(key)
        node = root
        WHILE node != null DO
            cmp = compare(key, node.key)
            IF cmp < 0 THEN
                node = node.left
            ELSE IF cmp > 0 THEN
                node = node.right
            ELSE
                RETURN node
            END IF
        END WHILE
        RETURN null
    END FUNCTION
    
    FUNCTION firstKey()
        IF root == null THEN
            ERROR "Map is empty"
        END IF
        node = root
        WHILE node.left != null DO
            node = node.left
        END WHILE
        RETURN node.key
    END FUNCTION
    
    FUNCTION lastKey()
        IF root == null THEN
            ERROR "Map is empty"
        END IF
        node = root
        WHILE node.right != null DO
            node = node.right
        END WHILE
        RETURN node.key
    END FUNCTION
    
    FUNCTION ceilingKey(key)
        // Return smallest key >= key
        node = root
        result = null
        
        WHILE node != null DO
            cmp = compare(key, node.key)
            IF cmp < 0 THEN
                result = node.key
                node = node.left
            ELSE IF cmp > 0 THEN
                node = node.right
            ELSE
                RETURN node.key
            END IF
        END WHILE
        
        RETURN result
    END FUNCTION
    
    FUNCTION floorKey(key)
        // Return largest key <= key
        node = root
        result = null
        
        WHILE node != null DO
            cmp = compare(key, node.key)
            IF cmp > 0 THEN
                result = node.key
                node = node.right
            ELSE IF cmp < 0 THEN
                node = node.left
            ELSE
                RETURN node.key
            END IF
        END WHILE
        
        RETURN result
    END FUNCTION
    
    FUNCTION subMap(fromKey, toKey)
        // Return entries with keys in [fromKey, toKey)
        result = []
        inOrderTraversal(root, fromKey, toKey, result)
        RETURN result
    END FUNCTION
    
    FUNCTION inOrderTraversal(node, start, end, result)
        IF node == null THEN
            RETURN
        END IF
        
        IF compare(node.key, start) > 0 THEN
            inOrderTraversal(node.left, start, end, result)
        END IF
        
        IF compare(node.key, start) >= 0 AND compare(node.key, end) < 0 THEN
            result.append((node.key, node.value))
        END IF
        
        IF compare(node.key, end) < 0 THEN
            inOrderTraversal(node.right, start, end, result)
        END IF
    END FUNCTION
    
    FUNCTION compare(a, b)
        IF a < b THEN RETURN -1
        ELSE IF a > b THEN RETURN 1
        ELSE RETURN 0
    END FUNCTION
END CLASS
```

**Complexity:** O(log n) for all operations.[3][1]

***

## 4. Set Operations

```text
FUNCTION union(setA, setB)
    result = SET()
    FOR element IN setA DO
        result.add(element)
    END FOR
    FOR element IN setB DO
        result.add(element)
    END FOR
    RETURN result
END FUNCTION
Complexity: O(|A| + |B|)

FUNCTION intersection(setA, setB)
    result = SET()
    smallerSet = setA IF len(setA) < len(setB) ELSE setB
    largerSet = setB IF len(setA) < len(setB) ELSE setA
    
    FOR element IN smallerSet DO
        IF element IN largerSet THEN
            result.add(element)
        END IF
    END FOR
    RETURN result
END FUNCTION
Complexity: O(min(|A|, |B|))

FUNCTION difference(setA, setB)
    result = SET()
    FOR element IN setA DO
        IF element NOT IN setB THEN
            result.add(element)
        END IF
    END FOR
    RETURN result
END FUNCTION
Complexity: O(|A|)

FUNCTION symmetricDifference(setA, setB)
    result = SET()
    FOR element IN setA DO
        IF element NOT IN setB THEN
            result.add(element)
        END IF
    END FOR
    FOR element IN setB DO
        IF element NOT IN setA THEN
            result.add(element)
        END IF
    END FOR
    RETURN result
END FUNCTION
Complexity: O(|A| + |B|)

FUNCTION isSubset(setA, setB)
    FOR element IN setA DO
        IF element NOT IN setB THEN
            RETURN false
        END IF
    END FOR
    RETURN true
END FUNCTION
Complexity: O(|A|)

FUNCTION isDisjoint(setA, setB)
    smallerSet = setA IF len(setA) < len(setB) ELSE setB
    largerSet = setB IF len(setA) < len(setB) ELSE setA
    
    FOR element IN smallerSet DO
        IF element IN largerSet THEN
            RETURN false
        END IF
    END FOR
    RETURN true
END FUNCTION
Complexity: O(min(|A|, |B|))
```

***

## 5. Advanced Data Structures

### a. Multiset (Frequency Map)

```text
CLASS Multiset
    map = HashMap()  // element -> count
    totalCount = 0
    
    FUNCTION add(element)
        count = map.get(element, 0)
        map.put(element, count + 1)
        totalCount += 1
    END FUNCTION
    
    FUNCTION remove(element)
        IF NOT map.containsKey(element) THEN
            RETURN false
        END IF
        
        count = map.get(element)
        IF count > 1 THEN
            map.put(element, count - 1)
        ELSE
            map.remove(element)
        END IF
        totalCount -= 1
        RETURN true
    END FUNCTION
    
    FUNCTION removeAll(element)
        IF NOT map.containsKey(element) THEN
            RETURN 0
        END IF
        
        count = map.get(element)
        map.remove(element)
        totalCount -= count
        RETURN count
    END FUNCTION
    
    FUNCTION count(element)
        RETURN map.get(element, 0)
    END FUNCTION
    
    FUNCTION contains(element)
        RETURN map.containsKey(element)
    END FUNCTION
    
    FUNCTION size()
        RETURN totalCount
    END FUNCTION
    
    FUNCTION uniqueElements()
        RETURN map.size()
    END FUNCTION
END CLASS
```

***

### b. Multimap (Multiple Values per Key)

```text
CLASS Multimap
    map = HashMap()  // key -> List<value>
    totalCount = 0
    
    FUNCTION put(key, value)
        IF NOT map.containsKey(key) THEN
            map.put(key, [])
        END IF
        map.get(key).append(value)
        totalCount += 1
    END FUNCTION
    
    FUNCTION get(key)
        RETURN map.get(key, [])
    END FUNCTION
    
    FUNCTION removeKey(key)
        IF NOT map.containsKey(key) THEN
            RETURN []
        END IF
        
        values = map.get(key)
        map.remove(key)
        totalCount -= len(values)
        RETURN values
    END FUNCTION
    
    FUNCTION removeValue(key, value)
        IF NOT map.containsKey(key) THEN
            RETURN false
        END IF
        
        values = map.get(key)
        IF value IN values THEN
            values.remove(value)
            totalCount -= 1
            IF len(values) == 0 THEN
                map.remove(key)
            END IF
            RETURN true
        END IF
        RETURN false
    END FUNCTION
    
    FUNCTION containsKey(key)
        RETURN map.containsKey(key)
    END FUNCTION
    
    FUNCTION containsEntry(key, value)
        RETURN value IN map.get(key, [])
    END FUNCTION
    
    FUNCTION keys()
        RETURN map.keys()
    END FUNCTION
    
    FUNCTION size()
        RETURN totalCount
    END FUNCTION
END CLASS
```

***

### c. Bidirectional Map (BiMap)

```text
CLASS BiMap
    forward = HashMap()  // key -> value
    reverse = HashMap()  // value -> key
    
    FUNCTION put(key, value)
        // Remove existing mappings
        IF forward.containsKey(key) THEN
            oldValue = forward.get(key)
            reverse.remove(oldValue)
        END IF
        IF reverse.containsKey(value) THEN
            oldKey = reverse.get(value)
            forward.remove(oldKey)
        END IF
        
        forward.put(key, value)
        reverse.put(value, key)
    END FUNCTION
    
    FUNCTION getByKey(key)
        RETURN forward.get(key)
    END FUNCTION
    
    FUNCTION getByValue(value)
        RETURN reverse.get(value)
    END FUNCTION
    
    FUNCTION removeByKey(key)
        IF NOT forward.containsKey(key) THEN
            RETURN null
        END IF
        
        value = forward.get(key)
        forward.remove(key)
        reverse.remove(value)
        RETURN value
    END FUNCTION
    
    FUNCTION removeByValue(value)
        IF NOT reverse.containsKey(value) THEN
            RETURN null
        END IF
        
        key = reverse.get(value)
        forward.remove(key)
        reverse.remove(value)
        RETURN key
    END FUNCTION
    
    FUNCTION containsKey(key)
        RETURN forward.containsKey(key)
    END FUNCTION
    
    FUNCTION containsValue(value)
        RETURN reverse.containsKey(value)
    END FUNCTION
    
    FUNCTION inverse()
        inverseBiMap = BiMap()
        inverseBiMap.forward = THIS.reverse
        inverseBiMap.reverse = THIS.forward
        RETURN inverseBiMap
    END FUNCTION
END CLASS
```

***

### d. LRU Cache (Map + Doubly-Linked List)

```text
CLASS LRUCache
    capacity = 0
    cache = HashMap()  // key -> Node
    head = Node(-1, -1)  // Dummy head (MRU)
    tail = Node(-1, -1)  // Dummy tail (LRU)
    
    CLASS Node
        key = 0
        value = 0
        prev = null
        next = null
        
        FUNCTION __init__(key, value)
            THIS.key = key
            THIS.value = value
        END FUNCTION
    END CLASS
    
    FUNCTION __init__(capacity)
        THIS.capacity = capacity
        head.next = tail
        tail.prev = head
    END FUNCTION
    
    FUNCTION get(key)
        IF NOT cache.containsKey(key) THEN
            RETURN -1
        END IF
        
        node = cache.get(key)
        moveToFront(node)
        RETURN node.value
    END FUNCTION
    
    FUNCTION put(key, value)
        IF cache.containsKey(key) THEN
            node = cache.get(key)
            node.value = value
            moveToFront(node)
        ELSE
            IF cache.size() >= capacity THEN
                // Evict LRU
                lru = tail.prev
                removeNode(lru)
                cache.remove(lru.key)
            END IF
            
            node = Node(key, value)
            cache.put(key, node)
            addToFront(node)
        END IF
    END FUNCTION
    
    FUNCTION moveToFront(node)
        removeNode(node)
        addToFront(node)
    END FUNCTION
    
    FUNCTION removeNode(node)
        node.prev.next = node.next
        node.next.prev = node.prev
    END FUNCTION
    
    FUNCTION addToFront(node)
        node.next = head.next
        node.prev = head
        head.next.prev = node
        head.next = node
    END FUNCTION
END CLASS
```

**Complexity:** O(1) for all operations.[1]

***

### e. Bloom Filter (Probabilistic Set)

```text
CLASS BloomFilter
    bits = BITARRAY(size)
    size = 0
    numHashFunctions = 0
    
    FUNCTION __init__(size, numHashFunctions)
        THIS.size = size
        THIS.numHashFunctions = numHashFunctions
        bits = BITARRAY(size, initialized to 0)
    END FUNCTION
    
    FUNCTION add(element)
        FOR i = 0 TO numHashFunctions - 1 DO
            idx = hash(element, i) % size
            bits[idx] = 1
        END FOR
    END FUNCTION
    
    FUNCTION contains(element)
        FOR i = 0 TO numHashFunctions - 1 DO
            idx = hash(element, i) % size
            IF bits[idx] == 0 THEN
                RETURN false  // Definitely NOT in set
            END IF
        END FOR
        RETURN true  // Probably in set (false positive possible)
    END FUNCTION
    
    FUNCTION hash(element, hashNum)
        // Generate different hash for each hashNum
        baseHash = djb2Hash(element.toString(), LARGE_PRIME)
        hash = (baseHash + hashNum * fnvHash(element.toString(), LARGE_PRIME)) % LARGE_PRIME
        RETURN hash
    END FUNCTION
    
    FUNCTION expectedFalsePositiveRate()
        // Formula: (1 - e^(-k*n/m))^k
        // k = numHashFunctions, n = items added, m = size
        n = estimatedItems
        k = numHashFunctions
        m = size
        RETURN POWER((1 - EXP(-k * n / m)), k)
    END FUNCTION
END CLASS
```

**Space:** O(m) bits  
**False positive rate:** Tunable by k and m.[4][1]

***

### f. Consistent Hashing (Distributed Map)

```text
CLASS ConsistentHash
    ring = TreeMap()  // hash(node) -> node
    numReplicas = 150
    
    FUNCTION addNode(node)
        FOR i = 0 TO numReplicas - 1 DO
            virtualKey = node.toString() + "_" + i.toString()
            hash = hashFunction(virtualKey)
            ring.put(hash, node)
        END FOR
    END FUNCTION
    
    FUNCTION removeNode(node)
        FOR i = 0 TO numReplicas - 1 DO
            virtualKey = node.toString() + "_" + i.toString()
            hash = hashFunction(virtualKey)
            ring.remove(hash)
        END FOR
    END FUNCTION
    
    FUNCTION getNode(key)
        IF ring.isEmpty() THEN
            RETURN null
        END IF
        
        hash = hashFunction(key)
        
        // Find next node on ring (clockwise)
        nodeHash = ring.ceilingKey(hash)
        
        IF nodeHash == null THEN
            nodeHash = ring.firstKey()  // Wrap around
        END IF
        
        RETURN ring.get(nodeHash)
    END FUNCTION
    
    FUNCTION hashFunction(str)
        RETURN murmurHash(str, LARGE_PRIME)
    END FUNCTION
END CLASS
```

**Benefit:** O(k/n) keys affected on node add/remove.[1]

***

### g. Disjoint Set (Union-Find)

```text
CLASS DisjointSet
    parent = HashMap()
    rank = HashMap()
    
    FUNCTION makeSet(element)
        parent.put(element, element)
        rank.put(element, 0)
    END FUNCTION
    
    FUNCTION find(element)
        // Path compression optimization
        IF parent.get(element) != element THEN
            parent.put(element, find(parent.get(element)))
        END IF
        RETURN parent.get(element)
    END FUNCTION
    
    FUNCTION union(elemA, elemB)
        rootA = find(elemA)
        rootB = find(elemB)
        
        IF rootA == rootB THEN
            RETURN  // Already in same set
        END IF
        
        // Union by rank optimization
        IF rank.get(rootA) < rank.get(rootB) THEN
            parent.put(rootA, rootB)
        ELSE IF rank.get(rootA) > rank.get(rootB) THEN
            parent.put(rootB, rootA)
        ELSE
            parent.put(rootB, rootA)
            rank.put(rootA, rank.get(rootA) + 1)
        END IF
    END FUNCTION
    
    FUNCTION isConnected(elemA, elemB)
        RETURN find(elemA) == find(elemB)
    END FUNCTION
    
    FUNCTION countComponents()
        roots = SET()
        FOR element IN parent.keys() DO
            roots.add(find(element))
        END FOR
        RETURN len(roots)
    END FUNCTION
END CLASS
```

**Complexity:** O(α(n)) ≈ O(1) amortized.[2][1]

***

## 6. Common Algorithm Patterns

### a. Two Sum

```text
FUNCTION twoSum(arr, target)
    numMap = HashMap()
    
    FOR i = 0 TO len(arr) - 1 DO
        complement = target - arr[i]
        
        IF numMap.containsKey(complement) THEN
            RETURN [numMap.get(complement), i]
        END IF
        
        numMap.put(arr[i], i)
    END FOR
    
    RETURN []
END FUNCTION
```

**Complexity:** O(n) time, O(n) space.[1]

***

### b. Group Anagrams

```text
FUNCTION groupAnagrams(words)
    anagramMap = HashMap()
    
    FOR word IN words DO
        sorted = SORT(word.toCharArray()).toString()
        
        IF NOT anagramMap.containsKey(sorted) THEN
            anagramMap.put(sorted, [])
        END IF
        
        anagramMap.get(sorted).append(word)
    END FOR
    
    result = []
    FOR key IN anagramMap.keys() DO
        result.append(anagramMap.get(key))
    END FOR
    
    RETURN result
END FUNCTION
```

**Complexity:** O(n × k log k) where k = word length.[1]

***

### c. Top K Frequent Elements

```text
FUNCTION topKFrequent(arr, k)
    freqMap = HashMap()
    FOR num IN arr DO
        freqMap.put(num, freqMap.get(num, 0) + 1)
    END FOR
    
    // Bucket sort by frequency
    buckets = ARRAY[len(arr) + 1]
    FOR num IN freqMap.keys() DO
        freq = freqMap.get(num)
        IF buckets[freq] == null THEN
            buckets[freq] = []
        END IF
        buckets[freq].append(num)
    END FOR
    
    result = []
    FOR i = len(buckets) - 1 DOWN TO 0 DO
        IF buckets[i] != null THEN
            FOR num IN buckets[i] DO
                result.append(num)
                IF len(result) == k THEN
                    RETURN result
                END IF
            END FOR
        END IF
    END FOR
    
    RETURN result
END FUNCTION
```

**Complexity:** O(n) time, O(n) space.[1]

---

### d. Longest Consecutive Sequence

```text
FUNCTION longestConsecutive(arr)
    numSet = SET(arr)
    maxLen = 0
    
    FOR num IN numSet DO
        // Only start counting from sequence start
        IF (num - 1) NOT IN numSet THEN
            currentNum = num
            currentLen = 1
            
            WHILE (currentNum + 1) IN numSet DO
                currentNum += 1
                currentLen += 1
            END WHILE
            
            maxLen = MAX(maxLen, currentLen)
        END IF
    END FOR
    
    RETURN maxLen
END FUNCTION
```

**Complexity:** O(n) time, O(n) space.[1]

***

### e. Subarray Sum Equals K

```text
FUNCTION subarraySum(arr, k)
    prefixSumMap = HashMap()
    prefixSumMap.put(0, 1)
    currentSum = 0
    count = 0
    
    FOR num IN arr DO
        currentSum += num
        
        IF prefixSumMap.containsKey(currentSum - k) THEN
            count += prefixSumMap.get(currentSum - k)
        END IF
        
        prefixSumMap.put(currentSum, prefixSumMap.get(currentSum, 0) + 1)
    END FOR
    
    RETURN count
END FUNCTION
```

**Complexity:** O(n) time, O(n) space.[1]

***

These pseudocode implementations provide the complete foundation for implementing sets, maps, and related algorithms in any programming language—covering everything from basic operations to advanced FAANG-level patterns![3][2][4][1]

[1](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/87948039/74a4f14c-ae28-40ab-aba5-10e9b4548725/PSEUDOCODE.md)
[2](https://www.geeksforgeeks.org/introduction-to-set-data-structure/)
[3](https://www.programiz.com/java-programming/treeset)
[4](https://courses.cs.washington.edu/courses/cse373/17wi/lectures/priority-queues.pdf)
[5](https://www.w3schools.com/dsa/dsa_intro.php)
[6](https://www.mta.ca/~rrosebru/oldcourse/263114/Dsa.pdf)
[7](https://users.csc.calpoly.edu/~jdalbey/SWE/pdl_std.html)
[8](https://www.codecademy.com/article/pseudocode-and-flowchart-complete-beginners-guide)
[9](https://ggnindia.dronacharya.info/Downloads/Sub-info/RelatedBook/4thSem/Object-Oriented-Programming-text-book-4.pdf)
[10](https://stackoverflow.com/questions/67742756/treeset-implementation)
[11](https://www.cs.cmu.edu/~15850/notes/cmu850-f20.pdf)