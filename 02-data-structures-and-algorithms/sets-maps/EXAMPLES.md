# 🟢 Examples: Sets & Maps – Essential, Advanced, and FAANG-Level Mastery

## 1. Basic Set Operations

```text
CLASS HashSet
    map = HashMap()  // Internal storage using map
    
    FUNCTION add(element)
        IF NOT map.containsKey(element) THEN
            map.put(element, true)
            RETURN true
        END IF
        RETURN false
    END FUNCTION
    
    FUNCTION remove(element)
        IF map.containsKey(element) THEN
            map.remove(element)
            RETURN true
        END IF
        RETURN false
    END FUNCTION
    
    FUNCTION contains(element)
        RETURN map.containsKey(element)
    END FUNCTION
    
    FUNCTION size()
        RETURN map.size()
    END FUNCTION
    
    FUNCTION isEmpty()
        RETURN map.size() == 0
    END FUNCTION
END CLASS
```

**Complexity:** O(1) average for all operations.[1][2]

***

## 2. Contains Duplicate (Set Membership)

```text
FUNCTION containsDuplicate(arr)
    // Check if array has any duplicates
    seen = SET()
    
    FOR num IN arr DO
        IF num IN seen THEN
            RETURN true
        END IF
        seen.add(num)
    END FOR
    
    RETURN false
END FUNCTION
```

**Example:** arr =[3][4][5]
- i=0: seen={1}
- i=1: seen={1, 2}
- i=2: seen={1, 2, 3}
- i=3: 1 already in seen → RETURN true

**Complexity:** O(n) time, O(n) space.[2]
**Interviewed at:** Google, Amazon, Facebook, Microsoft, Apple.[1][2]

---

## 3. Intersection of Two Arrays

```text
FUNCTION intersection(arr1, arr2)
    // Find elements that appear in both arrays
    set1 = SET(arr1)
    result = SET()
    
    FOR num IN arr2 DO
        IF num IN set1 THEN
            result.add(num)
        END IF
    END FOR
    
    RETURN list(result)
END FUNCTION
```

**Example:** arr1 =, arr2 =[4][3]
- set1 = {1, 2}
- Check 2: in set1 → add to result
- result = {2}
- RETURN[4]

**Complexity:** O(n + m) time, O(n) space.[2]
**Interviewed at:** Google, Microsoft.[6]

***

## 4. Longest Consecutive Sequence

```text
FUNCTION longestConsecutive(arr)
    // Find longest sequence of consecutive numbers
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

**Example:** arr =[5][7][3][4]
- numSet = {100, 4, 200, 1, 3, 2}
- Start at 1 (no 0 in set):
  - 1 → 2 → 3 → 4 (length 4)
- maxLen = 4

**Complexity:** O(n) time, O(n) space.[6][2]
**Interviewed at:** Google, Facebook, Amazon.[6]

***

## 5. Happy Number (Cycle Detection with Set)

```text
FUNCTION isHappyNumber(n)
    // Check if number eventually reaches 1
    seen = SET()
    
    WHILE n != 1 AND n NOT IN seen DO
        seen.add(n)
        n = sumOfSquaresOfDigits(n)
    END WHILE
    
    RETURN n == 1
END FUNCTION

FUNCTION sumOfSquaresOfDigits(n)
    sum = 0
    WHILE n > 0 DO
        digit = n % 10
        sum += digit * digit
        n = n / 10
    END WHILE
    RETURN sum
END FUNCTION
```

**Example:** n = 19
- 1² + 9² = 82
- 8² + 2² = 68
- 6² + 8² = 100
- 1² + 0² + 0² = 1 ✓

**Complexity:** O(log n) iterations.[1][2]
**Interviewed at:** Google, Amazon.[2]

***

## 6. Set Operations (Union, Intersection, Difference)

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

FUNCTION difference(setA, setB)
    // Elements in A but not in B
    result = SET()
    FOR element IN setA DO
        IF element NOT IN setB THEN
            result.add(element)
        END IF
    END FOR
    RETURN result
END FUNCTION

FUNCTION symmetricDifference(setA, setB)
    // (A - B) ∪ (B - A)
    return union(difference(setA, setB), difference(setB, setA))
END FUNCTION
```

**Example:**
- A = {1, 2, 3, 4}
- B = {3, 4, 5, 6}
- union(A, B) = {1, 2, 3, 4, 5, 6}
- intersection(A, B) = {3, 4}
- difference(A, B) = {1, 2}
- symmetricDifference(A, B) = {1, 2, 5, 6}

**Complexity:** O(n + m) for all operations.[6]

***

## 7. Two Sum (Map for Complement Lookup)

```text
FUNCTION twoSum(arr, target)
    // Find two numbers that sum to target
    numMap = {}  // num -> index
    
    FOR i = 0 TO len(arr)-1 DO
        complement = target - arr[i]
        IF complement IN numMap THEN
            RETURN [numMap[complement], i]
        END IF
        numMap[arr[i]] = i
    END FOR
    
    RETURN []  // Not found
END FUNCTION
```

**Example:** arr =, target = 9[8][9][10][4]
- i=0: complement=7, numMap={2:0}
- i=1: complement=2, found in numMap! RETURN[3]

**Complexity:** O(n) time, O(n) space.[1][2]
**Interviewed at:** Google, Amazon, Facebook, Microsoft, Apple.[2][1]

***

## 8. Group Anagrams (Map with Canonical Keys)

```text
FUNCTION groupAnagrams(words)
    // Group words that are anagrams of each other
    anagramMap = {}  // sorted_word -> list of words
    
    FOR word IN words DO
        // Canonical form: sorted characters
        sorted = SORT(word.toCharArray()).toString()
        IF sorted NOT IN anagramMap THEN
            anagramMap[sorted] = []
        END IF
        anagramMap[sorted].append(word)
    END FOR
    
    result = []
    FOR key IN anagramMap.keys() DO
        result.append(anagramMap[key])
    END FOR
    
    RETURN result
END FUNCTION
```

**Example:** ["eat", "tea", "tan", "ate", "nat", "bat"]
- sorted("eat") = "aet" → ["eat", "tea", "ate"]
- sorted("tan") = "ant" → ["tan", "nat"]
- sorted("bat") = "abt" → ["bat"]

**Complexity:** O(n × k log k) where k = word length.[1][2]
**Interviewed at:** Google, Microsoft, Amazon.[1]

***

## 9. Valid Anagram (Frequency Map)

```text
FUNCTION isAnagram(s, t)
    IF len(s) != len(t) THEN
        RETURN false
    END IF
    
    freqS = {}
    FOR char IN s DO
        freqS[char] = freqS.get(char, 0) + 1
    END FOR
    
    FOR char IN t DO
        IF char NOT IN freqS THEN
            RETURN false
        END IF
        freqS[char] -= 1
        IF freqS[char] == 0 THEN
            DELETE freqS[char]
        END IF
    END FOR
    
    RETURN len(freqS) == 0
END FUNCTION
```

**Example:** s = "anagram", t = "nagaram"
- freqS = {'a':3, 'n':1, 'g':1, 'r':1, 'm':1}
- After processing t: all counters reach 0
- RETURN true

**Complexity:** O(n) time, O(1) space (26 letters max).[2][1]
**Interviewed at:** Amazon, Microsoft.[2]

***

## 10. Top K Frequent Elements (Map + Heap)

```text
FUNCTION topKFrequent(arr, k)
    // Find K most frequent elements
    freqMap = {}  // num -> count
    FOR num IN arr DO
        freqMap[num] = freqMap.get(num, 0) + 1
    END FOR
    
    // Use min heap for top-K
    minHeap = MinHeap()
    FOR num, freq IN freqMap.items() DO
        minHeap.push((freq, num))
        IF minHeap.size() > k THEN
            minHeap.pop()
        END IF
    END FOR
    
    result = []
    WHILE NOT minHeap.isEmpty() DO
        freq, num = minHeap.pop()
        result.append(num)
    END WHILE
    
    RETURN result
END FUNCTION
```

**Alternative: Bucket Sort Approach**
```text
FUNCTION topKFrequentBucket(arr, k)
    freqMap = {}
    FOR num IN arr DO
        freqMap[num] = freqMap.get(num, 0) + 1
    END FOR
    
    // Bucket sort by frequency
    buckets = ARRAY[len(arr) + 1]
    FOR num, freq IN freqMap.items() DO
        IF buckets[freq] == null THEN
            buckets[freq] = []
        END IF
        buckets[freq].append(num)
    END FOR
    
    result = []
    FOR i = len(buckets)-1 DOWN TO 0 DO
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

**Example:** arr =, k = 2[5][3][4]
- freqMap: {1:3, 2:2, 3:1}
- Heap approach: Top 2:[3][4]
- Bucket approach: buckets=, buckets= →[4][5][3]

**Complexity:** O(n log k) heap, O(n) bucket.[6][1]
**Interviewed at:** Google, Amazon, Microsoft.[1]

---

## 11. Longest Substring Without Repeating Characters

```text
FUNCTION lengthOfLongestSubstring(s)
    charIndex = {}  // char -> last index
    maxLen = 0
    left = 0
    
    FOR right = 0 TO len(s)-1 DO
        IF s[right] IN charIndex AND charIndex[s[right]] >= left THEN
            left = charIndex[s[right]] + 1
        END IF
        charIndex[s[right]] = right
        maxLen = MAX(maxLen, right - left + 1)
    END FOR
    
    RETURN maxLen
END FUNCTION
```

**Example:** s = "abcabcbb"
- Sliding window with char index map
- "abc" → length 3 (max)
- "bca" → length 3
- "cab" → length 3
- maxLen = 3

**Complexity:** O(n) time, O(min(n, m)) space where m = alphabet size.[2][1]
**Interviewed at:** Google, Facebook, Microsoft, Amazon.[1]

***

## 12. Subarray Sum Equals K (Prefix Sum Map)

```text
FUNCTION subarraySum(arr, k)
    // Count subarrays with sum = k
    prefixSumMap = {0: 1}  // sum -> count
    currentSum = 0
    count = 0
    
    FOR num IN arr DO
        currentSum += num
        
        // Check if (currentSum - k) exists
        IF (currentSum - k) IN prefixSumMap THEN
            count += prefixSumMap[currentSum - k]
        END IF
        
        // Add current sum to map
        prefixSumMap[currentSum] = prefixSumMap.get(currentSum, 0) + 1
    END FOR
    
    RETURN count
END FUNCTION
```

**Example:** arr =, k = 2[3]
- i=0: currentSum=1, prefixSumMap={0:1, 1:1}, count=0
- i=1: currentSum=2, (2-2)=0 exists, count=1, prefixSumMap={0:1, 1:1, 2:1}
- i=2: currentSum=3, (3-2)=1 exists, count=2
- RETURN 2

**Complexity:** O(n) time, O(n) space.[6][2]
**Interviewed at:** Google, Facebook, Amazon.[6]

***

## 13. LRU Cache (Map + Doubly-Linked List)

```text
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

CLASS LRUCache
    capacity = 0
    cache = {}  // key -> Node
    head = Node(-1, -1)  // Dummy head (MRU)
    tail = Node(-1, -1)  // Dummy tail (LRU)
    
    FUNCTION __init__(capacity)
        THIS.capacity = capacity
        head.next = tail
        tail.prev = head
    END FUNCTION
    
    FUNCTION get(key)
        IF key NOT IN cache THEN
            RETURN -1
        END IF
        
        node = cache[key]
        moveToFront(node)
        RETURN node.value
    END FUNCTION
    
    FUNCTION put(key, value)
        IF key IN cache THEN
            node = cache[key]
            node.value = value
            moveToFront(node)
        ELSE
            IF len(cache) >= capacity THEN
                // Evict LRU
                lru = tail.prev
                removeNode(lru)
                DELETE cache[lru.key]
            END IF
            
            node = Node(key, value)
            cache[key] = node
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

**Example:**
```text
LRUCache cache = LRUCache(2)
cache.put(1, 1)  // cache: {1=1}
cache.put(2, 2)  // cache: {1=1, 2=2}
cache.get(1)     // returns 1, cache: {2=2, 1=1}
cache.put(3, 3)  // evicts key 2, cache: {1=1, 3=3}
cache.get(2)     // returns -1 (not found)
cache.put(4, 4)  // evicts key 1, cache: {3=3, 4=4}
```

**Complexity:** O(1) for all operations (get, put).[2][1]
**Interviewed at:** All FAANG companies.[1][2]

***

## 14. Design HashMap from Scratch

```text
CLASS MyHashMap
    buckets = ARRAY[1000]
    size = 1000
    
    FUNCTION _hash(key)
        RETURN key % size
    END FUNCTION
    
    FUNCTION put(key, value)
        idx = _hash(key)
        IF buckets[idx] == null THEN
            buckets[idx] = []
        END IF
        
        // Update if exists
        FOR pair IN buckets[idx] DO
            IF pair[0] == key THEN
                pair[1] = value
                RETURN
            END IF
        END FOR
        
        // Add new entry
        buckets[idx].append([key, value])
    END FUNCTION
    
    FUNCTION get(key)
        idx = _hash(key)
        IF buckets[idx] == null THEN
            RETURN -1
        END IF
        
        FOR pair IN buckets[idx] DO
            IF pair[0] == key THEN
                RETURN pair[1]
            END IF
        END FOR
        RETURN -1
    END FUNCTION
    
    FUNCTION remove(key)
        idx = _hash(key)
        IF buckets[idx] == null THEN
            RETURN
        END IF
        
        FOR i = 0 TO len(buckets[idx])-1 DO
            IF buckets[idx][i][0] == key THEN
                buckets[idx].removeAt(i)
                RETURN
            END IF
        END FOR
    END FUNCTION
END CLASS
```

**Complexity:** O(1) average, O(n/m) per operation where n = entries, m = buckets.[11][1]
**Interviewed at:** Google, Microsoft, Amazon.[1]

***

## 15. Word Pattern (Bidirectional Map)

```text
FUNCTION wordPattern(pattern, words)
    IF len(pattern) != len(words) THEN
        RETURN false
    END IF
    
    charToWord = {}  // pattern char -> word
    wordToChar = {}  // word -> pattern char
    
    FOR i = 0 TO len(pattern)-1 DO
        char = pattern[i]
        word = words[i]
        
        // Check char -> word mapping
        IF char IN charToWord THEN
            IF charToWord[char] != word THEN
                RETURN false
            END IF
        ELSE
            charToWord[char] = word
        END IF
        
        // Check word -> char mapping (bijection)
        IF word IN wordToChar THEN
            IF wordToChar[word] != char THEN
                RETURN false
            END IF
        ELSE
            wordToChar[word] = char
        END IF
    END FOR
    
    RETURN true
END FUNCTION
```

**Example:** pattern = "abba", words = ["dog", "cat", "cat", "dog"]
- a -> dog, dog -> a
- b -> cat, cat -> b
- b -> cat (consistent)
- a -> dog (consistent)
- RETURN true

**Complexity:** O(n) time, O(n) space.[2][1]
**Interviewed at:** Google, Amazon.[2]

***

## 16. Ransom Note (Frequency Check)

```text
FUNCTION canConstruct(ransomNote, magazine)
    magFreq = {}
    FOR char IN magazine DO
        magFreq[char] = magFreq.get(char, 0) + 1
    END FOR
    
    FOR char IN ransomNote DO
        IF char NOT IN magFreq OR magFreq[char] == 0 THEN
            RETURN false
        END IF
        magFreq[char] -= 1
    END FOR
    
    RETURN true
END FUNCTION
```

**Example:** ransomNote = "aa", magazine = "aab"
- magFreq = {'a': 2, 'b': 1}
- Use 'a': magFreq = {'a': 1, 'b': 1}
- Use 'a': magFreq = {'a': 0, 'b': 1}
- RETURN true

**Complexity:** O(n + m) time, O(1) space (26 letters).[1][2]
**Interviewed at:** Amazon, Microsoft.[2]

***

## 17. First Unique Character in String

```text
FUNCTION firstUniqChar(s)
    charCount = {}
    FOR char IN s DO
        charCount[char] = charCount.get(char, 0) + 1
    END FOR
    
    FOR i = 0 TO len(s)-1 DO
        IF charCount[s[i]] == 1 THEN
            RETURN i
        END IF
    END FOR
    
    RETURN -1
END FUNCTION
```

**Example:** s = "leetcode"
- charCount = {'l':1, 'e':3, 't':1, 'c':1, 'o':1, 'd':1}
- First unique: 'l' at index 0
- RETURN 0

**Complexity:** O(n) time, O(1) space (26 letters).[1][2]
**Interviewed at:** Google, Amazon, Microsoft.[1]

***

## 18. Isomorphic Strings (Character Mapping)

```text
FUNCTION isIsomorphic(s, t)
    IF len(s) != len(t) THEN
        RETURN false
    END IF
    
    mapS = {}  // s char -> t char
    mapT = {}  // t char -> s char
    
    FOR i = 0 TO len(s)-1 DO
        charS = s[i]
        charT = t[i]
        
        IF charS IN mapS THEN
            IF mapS[charS] != charT THEN
                RETURN false
            END IF
        ELSE
            mapS[charS] = charT
        END IF
        
        IF charT IN mapT THEN
            IF mapT[charT] != charS THEN
                RETURN false
            END IF
        ELSE
            mapT[charT] = charS
        END IF
    END FOR
    
    RETURN true
END FUNCTION
```

**Example:** s = "egg", t = "add"
- e -> a, a -> e
- g -> d, d -> g
- g -> d (consistent)
- RETURN true

**Complexity:** O(n) time, O(1) space (256 ASCII chars).[2]
**Interviewed at:** Amazon, Google.[2]

***

## 19. Four Sum II (Multi-Array Sum)

```text
FUNCTION fourSumCount(A, B, C, D)
    // Count tuples (i,j,k,l) where A[i]+B[j]+C[k]+D[l]=0
    sumAB = {}  // sum -> count
    
    // Build map of all A[i] + B[j] sums
    FOR a IN A DO
        FOR b IN B DO
            sumAB[a + b] = sumAB.get(a + b, 0) + 1
        END FOR
    END FOR
    
    count = 0
    // Check if -(C[k] + D[l]) exists in sumAB
    FOR c IN C DO
        FOR d IN D DO
            target = -(c + d)
            IF target IN sumAB THEN
                count += sumAB[target]
            END IF
        END FOR
    END FOR
    
    RETURN count
END FUNCTION
```

**Example:** A =, B = [-2,-1], C = [-1,2], D =[4][3]
- sumAB = {-1:1, 0:1, 1:1, 2:1}
- Check C+D pairs: count matches
- RETURN 2

**Complexity:** O(n²) time, O(n²) space.[6][2]
**Interviewed at:** Amazon, Facebook.[6]

***

## 20. Majority Element (Boyer-Moore + Map)

### Map Approach
```text
FUNCTION majorityElement(arr)
    // Find element appearing > n/2 times
    countMap = {}
    majority = len(arr) / 2
    
    FOR num IN arr DO
        countMap[num] = countMap.get(num, 0) + 1
        IF countMap[num] > majority THEN
            RETURN num
        END IF
    END FOR
    
    RETURN -1
END FUNCTION
```

### Boyer-Moore Voting (Optimal)
```text
FUNCTION majorityElementOptimal(arr)
    candidate = arr[0]
    count = 1
    
    FOR i = 1 TO len(arr)-1 DO
        IF count == 0 THEN
            candidate = arr[i]
            count = 1
        ELSE IF arr[i] == candidate THEN
            count += 1
        ELSE
            count -= 1
        END IF
    END FOR
    
    RETURN candidate
END FUNCTION
```

**Complexity:** O(n) time, O(n) space (map) or O(1) space (Boyer-Moore).[2]
**Interviewed at:** Google, Amazon.[2]

---

## 21. Valid Sudoku (Multiple Sets)

```text
FUNCTION isValidSudoku(board)
    rows = ARRAY[9] of SET()
    cols = ARRAY[9] of SET()
    boxes = ARRAY[9] of SET()
    
    FOR i = 0 TO 8 DO
        FOR j = 0 TO 8 DO
            IF board[i][j] == '.' THEN
                CONTINUE
            END IF
            
            num = board[i][j]
            boxIndex = (i / 3) * 3 + (j / 3)
            
            IF num IN rows[i] OR num IN cols[j] OR num IN boxes[boxIndex] THEN
                RETURN false
            END IF
            
            rows[i].add(num)
            cols[j].add(num)
            boxes[boxIndex].add(num)
        END FOR
    END FOR
    
    RETURN true
END FUNCTION
```

**Complexity:** O(1) time (fixed 9×9), O(1) space.[6]
**Interviewed at:** Google, Microsoft, Amazon.[6]

***

## 22. Minimum Window Substring (Map + Sliding Window)

```text
FUNCTION minWindow(s, t)
    IF len(s) < len(t) THEN
        RETURN ""
    END IF
    
    tFreq = {}
    FOR char IN t DO
        tFreq[char] = tFreq.get(char, 0) + 1
    END FOR
    
    required = len(tFreq)
    formed = 0
    windowCounts = {}
    
    left = 0
    minLen = INFINITY
    minLeft = 0
    
    FOR right = 0 TO len(s)-1 DO
        char = s[right]
        windowCounts[char] = windowCounts.get(char, 0) + 1
        
        IF char IN tFreq AND windowCounts[char] == tFreq[char] THEN
            formed += 1
        END IF
        
        WHILE left <= right AND formed == required DO
            // Update minimum window
            IF right - left + 1 < minLen THEN
                minLen = right - left + 1
                minLeft = left
            END IF
            
            // Shrink window
            char = s[left]
            windowCounts[char] -= 1
            IF char IN tFreq AND windowCounts[char] < tFreq[char] THEN
                formed -= 1
            END IF
            left += 1
        END WHILE
    END FOR
    
    RETURN "" IF minLen == INFINITY ELSE s[minLeft:minLeft+minLen]
END FUNCTION
```

**Example:** s = "ADOBECODEBANC", t = "ABC"
- Minimum window: "BANC"

**Complexity:** O(|s| + |t|) time, O(|s| + |t|) space [2].  
**Interviewed at:** Google, Facebook, Amazon.[2]

---

## 23. Logger Rate Limiter (Timestamp Map)

```text
CLASS Logger
    messageTime = {}  // message -> last timestamp
    
    FUNCTION shouldPrintMessage(timestamp, message)
        IF message NOT IN messageTime THEN
            messageTime[message] = timestamp
            RETURN true
        END IF
        
        IF timestamp - messageTime[message] >= 10 THEN
            messageTime[message] = timestamp
            RETURN true
        END IF
        
        RETURN false
    END FUNCTION
END CLASS
```

**Example:**
```text
logger.shouldPrintMessage(1, "foo") → true
logger.shouldPrintMessage(2, "bar") → true
logger.shouldPrintMessage(3, "foo") → false (within 10s)
logger.shouldPrintMessage(8, "bar") → false (within 10s)
logger.shouldPrintMessage(11, "foo") → true (>= 10s)
```

**Complexity:** O(1) per call.[2]
**Interviewed at:** Google, Amazon.[2]

***

## 24. Jewels and Stones (Set Lookup)

```text
FUNCTION numJewelsInStones(jewels, stones)
    jewelSet = SET(jewels)
    count = 0
    
    FOR stone IN stones DO
        IF stone IN jewelSet THEN
            count += 1
        END IF
    END FOR
    
    RETURN count
END FUNCTION
```

**Example:** jewels = "aA", stones = "aAAbbbb"
- jewelSet = {'a', 'A'}
- Count 'a' and 'A' in stones: 3
- RETURN 3

**Complexity:** O(j + s) time, O(j) space.[2]
**Interviewed at:** Google.[2]

***

## 25. Bloom Filter Implementation (Probabilistic Set)

```text
CLASS BloomFilter
    bits = BITARRAY(size)
    size = 0
    numHashFunctions = 0
    hashFunctions = []
    
    FUNCTION __init__(size, numHashFunctions)
        THIS.size = size
        THIS.numHashFunctions = numHashFunctions
        THIS.bits = BITARRAY(size)
        
        // Initialize multiple hash functions
        FOR i = 0 TO numHashFunctions-1 DO
            THIS.hashFunctions.append(createHashFunction(i))
        END FOR
    END FUNCTION
    
    FUNCTION add(element)
        FOR hashFunc IN hashFunctions DO
            idx = hashFunc(element) % size
            bits[idx] = 1
        END FOR
    END FUNCTION
    
    FUNCTION contains(element)
        FOR hashFunc IN hashFunctions DO
            idx = hashFunc(element) % size
            IF bits[idx] == 0 THEN
                RETURN false  // Definitely NOT in set
            END IF
        END FOR
        RETURN true  // PROBABLY in set (false positive possible)
    END FUNCTION
    
    FUNCTION createHashFunction(seed)
        RETURN FUNCTION(element)
            hash = seed
            FOR char IN element DO
                hash = (hash * 31 + ASCII(char)) % LARGE_PRIME
            END FOR
            RETURN hash
        END FUNCTION
    END FUNCTION
END CLASS
```

**Example:**
```text
bloom = BloomFilter(1000, 3)
bloom.add("apple")
bloom.add("banana")
bloom.contains("apple") → true
bloom.contains("orange") → false or true (false positive)
```

**Space:** O(m) bits for n elements (much smaller than hash set).[1][2]
**False positive rate:** Tunable by number of hash functions.[1]
**Interviewed at:** Google, Amazon.[1]

***

## 26. Consistent Hashing (Distributed Map)

```text
CLASS ConsistentHash
    ring = TreeMap()  // hash -> node
    numReplicas = 150
    
    FUNCTION addNode(node)
        FOR i = 0 TO numReplicas-1 DO
            hash = hashFunction(node.toString() + "_" + i.toString())
            ring.put(hash, node)
        END FOR
    END FUNCTION
    
    FUNCTION removeNode(node)
        FOR i = 0 TO numReplicas-1 DO
            hash = hashFunction(node.toString() + "_" + i.toString())
            ring.remove(hash)
        END FOR
    END FUNCTION
    
    FUNCTION getNode(key)
        IF ring.isEmpty() THEN
            RETURN null
        END IF
        
        hash = hashFunction(key)
        
        // Find next node on ring (clockwise)
        entry = ring.ceilingEntry(hash)
        
        IF entry == null THEN
            entry = ring.firstEntry()  // Wrap around
        END IF
        
        RETURN entry.value
    END FUNCTION
    
    FUNCTION hashFunction(str)
        hash = 0
        FOR char IN str DO
            hash = (hash * 31 + ASCII(char)) % LARGE_PRIME
        END FOR
        RETURN hash
    END FUNCTION
END CLASS
```

**Example:**
```text
ch = ConsistentHash()
ch.addNode("Server1")
ch.addNode("Server2")
ch.addNode("Server3")

ch.getNode("key1") → "Server2"
ch.getNode("key2") → "Server3"
ch.getNode("key3") → "Server1"

ch.removeNode("Server2")
// Only keys mapped to Server2 get remapped
```

**Benefit:** Adding/removing nodes only affects O(k/n) of keys.[1]
**Complexity:** O(log n) per lookup where n = number of virtual nodes.[1]
**Interviewed at:** Amazon, Google, Netflix.[1]

***

## 27. Disjoint Set Union-Find (Set Connectivity)

```text
CLASS DisjointSet
    parent = {}
    rank = {}
    
    FUNCTION makeSet(element)
        parent[element] = element
        rank[element] = 0
    END FUNCTION
    
    FUNCTION find(element)
        // Path compression optimization
        IF parent[element] != element THEN
            parent[element] = find(parent[element])
        END IF
        RETURN parent[element]
    END FUNCTION
    
    FUNCTION union(elemA, elemB)
        rootA = find(elemA)
        rootB = find(elemB)
        
        IF rootA == rootB THEN
            RETURN  // Already in same set
        END IF
        
        // Union by rank optimization
        IF rank[rootA] < rank[rootB] THEN
            parent[rootA] = rootB
        ELSE IF rank[rootA] > rank[rootB] THEN
            parent[rootB] = rootA
        ELSE
            parent[rootB] = rootA
            rank[rootA] += 1
        END IF
    END FUNCTION
    
    FUNCTION isConnected(elemA, elemB)
        RETURN find(elemA) == find(elemB)
    END FUNCTION
END CLASS
```

**Example: Number of Connected Components**
```text
FUNCTION countComponents(n, edges)
    ds = DisjointSet()
    
    // Make sets for all nodes
    FOR i = 0 TO n-1 DO
        ds.makeSet(i)
    END FOR
    
    // Union connected nodes
    FOR edge IN edges DO
        ds.union(edge[0], edge[1])
    END FOR
    
    // Count unique roots
    roots = SET()
    FOR i = 0 TO n-1 DO
        roots.add(ds.find(i))
    END FOR
    
    RETURN len(roots)
END FUNCTION
```

**Complexity:** O(α(n)) ≈ O(1) amortized for find/union.[12][13]
**Applications:** Kruskal's MST, network connectivity, cycle detection.[13][12]
**Interviewed at:** Google, Amazon, Facebook.[6]

***

## 28. Palindrome Pairs (Map with Reverse Lookup)

```text
FUNCTION palindromePairs(words)
    wordMap = {}  // word -> index
    FOR i = 0 TO len(words)-1 DO
        wordMap[words[i]] = i
    END FOR
    
    result = []
    
    FOR i = 0 TO len(words)-1 DO
        word = words[i]
        
        FOR j = 0 TO len(word) DO
            prefix = word[0:j]
            suffix = word[j:len(word)]
            
            // If prefix is palindrome, check if reverse of suffix exists
            IF isPalindrome(prefix) THEN
                reversedSuffix = reverse(suffix)
                IF reversedSuffix IN wordMap AND wordMap[reversedSuffix] != i THEN
                    result.append([wordMap[reversedSuffix], i])
                END IF
            END IF
            
            // If suffix is palindrome, check if reverse of prefix exists
            IF j != len(word) AND isPalindrome(suffix) THEN
                reversedPrefix = reverse(prefix)
                IF reversedPrefix IN wordMap AND wordMap[reversedPrefix] != i THEN
                    result.append([i, wordMap[reversedPrefix]])
                END IF
            END IF
        END FOR
    END FOR
    
    RETURN result
END FUNCTION

FUNCTION isPalindrome(s)
    left = 0
    right = len(s) - 1
    WHILE left < right DO
        IF s[left] != s[right] THEN
            RETURN false
        END IF
        left += 1
        right -= 1
    END WHILE
    RETURN true
END FUNCTION
```

**Complexity:** O(n × k²) where n = number of words, k = average word length.[2]
**Interviewed at:** Google, Amazon.[2]

***

## 29. Time-Based Key-Value Store (Map of Maps)

```text
CLASS TimeMap
    store = {}  // key -> list of (timestamp, value)
    
    FUNCTION set(key, value, timestamp)
        IF key NOT IN store THEN
            store[key] = []
        END IF
        store[key].append((timestamp, value))
    END FUNCTION
    
    FUNCTION get(key, timestamp)
        IF key NOT IN store THEN
            RETURN ""
        END IF
        
        values = store[key]
        
        // Binary search for largest timestamp <= given timestamp
        left = 0
        right = len(values) - 1
        result = ""
        
        WHILE left <= right DO
            mid = (left + right) / 2
            IF values[mid][0] <= timestamp THEN
                result = values[mid][1]
                left = mid + 1
            ELSE
                right = mid - 1
            END IF
        END WHILE
        
        RETURN result
    END FUNCTION
END CLASS
```

**Example:**
```text
timeMap.set("foo", "bar", 1)
timeMap.get("foo", 1) → "bar"
timeMap.get("foo", 3) → "bar"
timeMap.set("foo", "bar2", 4)
timeMap.get("foo", 4) → "bar2"
timeMap.get("foo", 5) → "bar2"
```

**Complexity:** set O(1), get O(log n).[2]
**Interviewed at:** Google, Amazon.[2]

***

## 30. Count Primes (Sieve of Eratosthenes with Set)

```text
FUNCTION countPrimes(n)
    IF n <= 2 THEN
        RETURN 0
    END IF
    
    isPrime = ARRAY[n] filled with true
    isPrime[0] = false
    isPrime[1] = false
    
    FOR i = 2 TO SQRT(n) DO
        IF isPrime[i] THEN
            // Mark all multiples as not prime
            FOR j = i*i TO n-1 STEP i DO
                isPrime[j] = false
            END FOR
        END IF
    END FOR
    
    count = 0
    FOR i = 2 TO n-1 DO
        IF isPrime[i] THEN
            count += 1
        END IF
    END FOR
    
    RETURN count
END FUNCTION
```

**Alternative: Using Set**
```text
FUNCTION countPrimesSet(n)
    IF n <= 2 THEN
        RETURN 0
    END IF
    
    primes = SET()
    FOR i = 2 TO n-1 DO
        primes.add(i)
    END FOR
    
    FOR i = 2 TO SQRT(n) DO
        IF i IN primes THEN
            // Remove all multiples
            FOR j = i*i TO n-1 STEP i DO
                primes.remove(j)
            END FOR
        END IF
    END FOR
    
    RETURN len(primes)
END FUNCTION
```

**Complexity:** O(n log log n) time, O(n) space.[6]
**Interviewed at:** Amazon, Microsoft.[6]

***

These examples cover **foundational to advanced set and map patterns**—master them for FAANG interviews, system design, and real-world engineering challenges![11][6][1][2]

[1](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/87948039/2828d5a0-a1a7-4ada-beb1-d9d9ad9182eb/EXAMPLES.md)
[2](https://igotanoffer.com/blogs/tech/map-interview-questions)
[3](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/87948039/74a4f14c-ae28-40ab-aba5-10e9b4548725/PSEUDOCODE.md)
[4](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/87948039/e8d1ce7d-f58b-4d61-af51-99ebdc6fb2cc/COLLISION.md)
[5](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/87948039/be8cb354-7753-435a-9511-201a41ae0036/DESCRIPTION.md)
[6](https://www.geeksforgeeks.org/dsa/top-100-data-structure-and-algorithms-dsa-interview-questions-topic-wise/)
[7](https://www.geeksforgeeks.org/dsa/introduction-to-set-data-structure/)
[8](https://www.interviewbit.com/data-structure-interview-questions/)
[9](https://www.usdsi.org/data-science-insights/understanding-data-structures-in-2025)
[10](https://blog.logrocket.com/javascript-maps-vs-sets-choosing-your-data-structure/)
[11](https://www.interviewbit.com/hashmap-interview-questions/)
[12](https://blog.heycoach.in/exploring-advanced-algorithms-and-data-structures/)
[13](https://www.shine.com/blog/data-structure-interview-questions)
[14](https://gateoverflow.in/460057/gate-cse-2025-set-1-question-23)
[15](https://www.scribd.com/document/839900847/Data-Structure-DS-Important-Questions-2025)
[16](https://unstop.com/blog/data-structure-interview-questions)
[17](https://javascript.info/map-set)