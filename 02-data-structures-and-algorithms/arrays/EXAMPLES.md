🟢 Examples: Arrays – From Basics to FAANG-Level Mastery

1. Basic Array Declaration and Access
   text
   // Fixed-length array
   int[] scores = [98, 87, 92, 78, 100]
   print(scores[2]) // Output: 92

// Dynamic/growable array (List in C#/Java, ArrayList, Python list)
List<string> students = []
students.add("Akram")
students.add("Sara")
print(students[0]) // Output: "Akram" 2. Multi-dimensional Array (Matrix)
text
// 2D array for classroom seating
string[,] seating = [
["A1", "A2", "A3"],
["B1", "B2", "B3"]
]
print(seating[1][2]) // Output: "B3" 3. Iterating Through Arrays
text
// Sum all values in an array
int sum = 0
for i = 0 to length(scores)-1:
sum += scores[i]
print(sum)
Real World: Computing total sales, attendance, energy usage from daily logs.

4. Array Slice/Subarray
   text
   // Extract a subarray
   int[] transactions = [100, 200, 150, 400, 350]
   int[] lastThree = transactions[2:5] // [150, 400, 350]
5. Two Pointer Technique (FAANG: Remove Duplicates from Sorted Array)
   text
   function removeDuplicates(nums):
   if nums.length == 0: return 0
   int write = 1
   for read = 1 to nums.length-1:
   if nums[read] != nums[write-1]:
   nums[write] = nums[read]
   write += 1
   return write
   Interviewed at: Google, Facebook, Amazon

6. Sliding Window Pattern (Maximum Sum Subarray of Size K)
   text
   function maxSumSubarray(arr, k):
   int maxSum = 0, windowSum = 0
   for i = 0 to k-1:
   windowSum += arr[i]
   maxSum = windowSum
   for i = k to arr.length-1:
   windowSum += arr[i] - arr[i-k]
   maxSum = max(maxSum, windowSum)
   return maxSum
   Interviewed at: Amazon, Netflix, Salesforce
   Leetcode: #Maximum Average Subarray I/II

7. Prefix Sum (Range Sum Query Optimization)
   text
   function prefixSums(arr):
   prefix = [0]
   for num in arr:
   prefix.append(prefix[-1] + num)
   return prefix

// Range sum query (i, j): prefix[j+1] - prefix[i]
Used in: Analytics, Finance, ML
Leetcode: #Range Sum Query - Immutable

8. Array Rotation (Leetcode/FAANG)
   text
   // Rotate an array right by k steps (cyclically)
   function rotate(nums, k):
   k = k % nums.length
   reverse(nums, 0, nums.length-1)
   reverse(nums, 0, k-1)
   reverse(nums, k, nums.length-1)
   Interviewed at: Microsoft, Facebook
   Leetcode: #Rotate Array

9. Binary Search (Classic) in Sorted Array
   text
   function binarySearch(arr, target):
   left, right = 0, arr.length-1
   while left <= right:
   mid = left + (right-left)//2
   if arr[mid] == target:
   return mid
   elif arr[mid] < target:
   left = mid+1
   else:
   right = mid-1
   return -1
   Used in: Database query optimizers, autocomplete, search engines
   Leetcode: #Binary Search
   Interviewed at: Google, LinkedIn

10. Merge Intervals (Array of Arrays)
    text
    function mergeIntervals(intervals):
    sort intervals based on start time
    merged = []
    for interval in intervals:
    if merged.isEmpty() or merged[-1][1] < interval[0]:
    merged.append(interval)
    else:
    merged[-1][1] = max(merged[-1][1], interval[1])
    return merged
    Interviewed at: Facebook, Amazon
    Leetcode: #Merge Intervals

11. Kadane’s Algorithm (Maximum Subarray Problem)
    text
    function maxSubArray(nums):
    maxCurr = maxGlobal = nums[0]
    for i = 1 to nums.length-1:
    maxCurr = max(nums[i], maxCurr + nums[i])
    maxGlobal = max(maxGlobal, maxCurr)
    return maxGlobal
    Real-world: Stock price analytics, streaming log analysis
    Leetcode: #Maximum Subarray
    Interviewed at: Amazon, Google

12. Product of Array Except Self (Without Division)
    text
    function productExceptSelf(nums):
    n = nums.length
    answer = [1] _ n
    left = 1
    for i=0 to n-1:
    answer[i] = left
    left _= nums[i]
    right = 1
    for i=n-1 to 0 step -1:
    answer[i] _= right
    right _= nums[i]
    return answer
    Leetcode: #238
    Interviewed at: Amazon, Facebook

13. Find All Duplicates in an Array (Read Only, O(1) Space, O(n) Time)
    text
    function findDuplicates(nums):
    res = []
    for i = 0 to nums.length-1:
    idx = abs(nums[i]) - 1
    if nums[idx] < 0:
    res.append(abs(nums[i]))
    else:
    nums[idx] = -nums[idx]
    return res
    Leetcode: #442
    Interviewed at: Google, Uber

14. Real-World System Example: In-memory Leaderboard
    text
    // Top 10 users by score in real-time game
    int[] topScores = new int[10]
    for each scoreEvent in realTimeFeed:
    insertAndKeepTop10(topScores, scoreEvent.score)
    Used in: Gaming, Stock rankings, Social network trending feeds

15. Real-World System Example: Image Processing
    text
    // 2D array (matrix) holds pixel brightness (e.g., grayscale)
    int[,] pixels = new int[height][width]
    for x in 0 to width-1:
    for y in 0 to height-1:
    adjustContrast(pixels[y][x])
    Used in: Photo editing, computer vision, medical imaging (MRI scans)

16. Error/Pitfall Example: Off-by-One
    text
    int[] arr = [1,2,3,4,5]
    for i=0 to arr.length: // ERROR: arr[arr.length] is out of bounds!
    print(arr[i])
17. Sparse Array Representation (Large Datasets)
    text
    // Dictionary only stores non-zero/defaults;
    sparseArr = {2: "A", 1000007: "X"} // Most values are implicit defaults
    Used in: ML, Graph processing, Simulation
