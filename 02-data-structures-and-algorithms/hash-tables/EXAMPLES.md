🟢 Examples: Hash Tables – Essential, Advanced, and FAANG-Level Mastery
1. Basic Hash Table Operations
text
CLASS HashTable
    buckets = ARRAY[size]
    size = 16
    
    FUNCTION hash(key)
        // Simple hash: sum of characters modulo size
        hashVal = 0
        FOR char IN key DO
            hashVal += ASCII(char)
        END FOR
        RETURN hashVal % size
    END FUNCTION
    
    FUNCTION put(key, value)
        idx = hash(key)
        IF buckets[idx] == null THEN
            buckets[idx] = []
        END IF
        // Separate chaining: store (key, value) pairs
        FOR i = 0 TO len(buckets[idx])-1 DO
            IF buckets[idx][i].key == key THEN
                buckets[idx][i].value = value  // Update
                RETURN
            END IF
        END FOR
        buckets[idx].append((key, value))
    END FUNCTION
    
    FUNCTION get(key)
        idx = hash(key)
        IF buckets[idx] == null THEN
            RETURN null
        END IF
        FOR pair IN buckets[idx] DO
            IF pair.key == key THEN
                RETURN pair.value
            END IF
        END FOR
        RETURN null
    END FUNCTION
    
    FUNCTION remove(key)
        idx = hash(key)
        IF buckets[idx] == null THEN
            RETURN
        END IF
        FOR i = 0 TO len(buckets[idx])-1 DO
            IF buckets[idx][i].key == key THEN
                buckets[idx].removeAt(i)
                RETURN
            END IF
        END FOR
    END FUNCTION
END CLASS
Complexity: O(1) average, O(n) worst-case.

2. Two Sum Problem (Hash Table)
text
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
Example: arr=, target=9

i=0: complement=7, numMap={2:0}

i=1: complement=2, found in numMap! Return

Complexity: O(n) time, O(n) space.
Interviewed at: Google, Amazon, Facebook, Microsoft, Apple.

3. Anagrams (Frequency Hash)
text
FUNCTION groupAnagrams(words)
    // Group words that are anagrams of each other
    anagramMap = {}  // sorted_word -> list of words
    
    FOR word IN words DO
        // Canonical form: sorted characters
        sorted = SORT(word)
        IF sorted NOT IN anagramMap THEN
            anagramMap[sorted] = []
        END IF
        anagramMap[sorted].append(word)
    END FOR
    
    result = []
    FOR sorted IN anagramMap DO
        result.append(anagramMap[sorted])
    END FOR
    
    RETURN result
END FUNCTION
Example: ["eat", "tea", "ate", "bat", "tab"]

sorted("eat") = "aet" → ["eat", "tea", "ate"]

sorted("bat") = "abt" → ["bat", "tab"]

Complexity: O(n * k log k) where k = word length.
Interviewed at: Google, Microsoft.

4. Two Sum II (Multiple Pairs)
text
FUNCTION twoSumAllPairs(arr, target)
    // Find all unique pairs summing to target
    result = []
    numMap = {}  // num -> count
    
    FOR num IN arr DO
        numMap[num] = numMap.get(num, 0) + 1
    END FOR
    
    FOR num IN numMap.keys() DO
        complement = target - num
        IF complement IN numMap THEN
            IF num < complement THEN
                result.append([num, complement])
            ELSE IF num == complement AND numMap[num] > 1 THEN
                result.append([num, num])
            END IF
        END IF
    END FOR
    
    RETURN result
END FUNCTION
Complexity: O(n) time.

5. LRU Cache (Hash Table + Doubly-Linked List)
text
CLASS LRUCache
    capacity = 0
    cache = {}  // key -> value
    order = DoublyLinkedList()  // Track access order
    keyToNode = {}  // key -> node in order
    
    FUNCTION __init__(capacity)
        THIS.capacity = capacity
    END FUNCTION
    
    FUNCTION get(key)
        IF key NOT IN cache THEN
            RETURN -1
        END IF
        // Move to front (most recent)
        order.moveToFront(keyToNode[key])
        RETURN cache[key]
    END FUNCTION
    
    FUNCTION put(key, value)
        IF key IN cache THEN
            cache[key] = value
            order.moveToFront(keyToNode[key])
        ELSE
            IF len(cache) >= capacity THEN
                // Evict least recently used (back)
                lruKey = order.removeLast()
                DELETE cache[lruKey]
                DELETE keyToNode[lruKey]
            END IF
            cache[key] = value
            node = order.addFront(key)
            keyToNode[key] = node
        END IF
    END FUNCTION
END CLASS
Complexity: O(1) for all operations (get, put).
Interviewed at: All FAANG.

6. Frequency Counter (Top-K Elements)
text
FUNCTION topKFrequent(arr, k)
    // Find K most frequent elements
    freqMap = {}  // num -> count
    FOR num IN arr DO
        freqMap[num] = freqMap.get(num, 0) + 1
    END FOR
    
    // Use heap for top-K
    minHeap = MinHeap()
    FOR num, freq IN freqMap DO
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
Example: arr=, k=2

freqMap: {1:3, 2:2, 3:1}

Top 2:

Complexity: O(n log k).
Interviewed at: Google, Amazon, Microsoft.

7. Design HashMap from Scratch
text
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
        FOR pair IN buckets[idx] DO
            IF pair[0] == key THEN
                pair[1] = value
                RETURN
            END IF
        END FOR
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
Interviewed at: Google, Microsoft, Amazon.

8. Valid Anagram (Hash Frequency)
text
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
Complexity: O(n).

9. Longest Substring Without Repeating Characters
text
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
Example: s="abcabcbb"

Sliding window with hash of characters

maxLen = 3 ("abc")

Complexity: O(n).
Interviewed at: Google, Facebook, Microsoft.

10. Happy Number (Cycle Detection)
text
FUNCTION isHappyNumber(n)
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
        n /= 10
    END WHILE
    RETURN sum
END FUNCTION
Example: n=19

1² + 9² = 82

8² + 2² = 68

...eventually reaches 1

Complexity: O(log n) iterations.

11. Bloom Filter (Probabilistic Set)
text
CLASS BloomFilter
    bits = BITARRAY(size)
    hashFunctions = [hash1, hash2, hash3]
    
    FUNCTION add(element)
        FOR hashFunc IN hashFunctions DO
            idx = hashFunc(element) % size
            bits[idx] = true
        END FOR
    END FUNCTION
    
    FUNCTION contains(element)
        FOR hashFunc IN hashFunctions DO
            idx = hashFunc(element) % size
            IF NOT bits[idx] THEN
                RETURN false  // Definitely not in set
            END IF
        END FOR
        RETURN true  // Probably in set (false positive possible)
    END FUNCTION
END CLASS
Space: O(m) bits for n elements (much smaller than hash table).
False positive rate: Tunable by number of hash functions.
Interviewed at: Google, Amazon.

12. Consistent Hashing (Distributed Systems)
text
CLASS ConsistentHash
    ring = {}  // hash(node) -> node
    sortedKeys = []
    
    FUNCTION addNode(node)
        FOR i = 0 TO numReplicas-1 DO
            hash = hash(node + i.toString())
            ring[hash] = node
            sortedKeys.append(hash)
        END FOR
        SORT(sortedKeys)
    END FUNCTION
    
    FUNCTION removeNode(node)
        FOR i = 0 TO numReplicas-1 DO
            hash = hash(node + i.toString())
            DELETE ring[hash]
            sortedKeys.removeValue(hash)
        END FOR
    END FUNCTION
    
    FUNCTION getNode(key)
        hash = hash(key)
        // Binary search for next node >= hash
        idx = binarySearchLeftmost(sortedKeys, hash)
        IF idx == len(sortedKeys) THEN
            idx = 0  // Wrap around
        END IF
        RETURN ring[sortedKeys[idx]]
    END FUNCTION
END CLASS
Benefit: Adding/removing nodes only affects O(k/n) of keys (k=total, n=nodes).
Interviewed at: Amazon, Google.

13. Word Pattern (Hash + Bijection)
text
FUNCTION wordPattern(pattern, words)
    IF len(pattern) != len(words) THEN
        RETURN false
    END IF
    
    charToWord = {}  // pattern char -> word
    wordToChar = {}  // word -> pattern char
    
    FOR i = 0 TO len(pattern)-1 DO
        char = pattern[i]
        word = words[i]
        
        IF char IN charToWord THEN
            IF charToWord[char] != word THEN
                RETURN false
            END IF
        ELSE
            charToWord[char] = word
        END IF
        
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
Complexity: O(n).

14. Ransomware / Ransom Note (Frequency Check)
text
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
Complexity: O(n + m).

15. First Unique Character in String
text
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
Complexity: O(n).
Interviewed at: Google, Amazon, Microsoft.

These examples cover foundational to advanced hash table patterns—master them for FAANG interviews, system design, and real-world engineering challenges!