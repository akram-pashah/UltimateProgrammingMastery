🟢 Interactive Logging: Arrays – Operation-by-Operation Trace

1. Array Declaration and Access
   text
   [Declare] int[] arr = [7, 3, 9, 2, 8]
   [Allocate] Memory blocks 0xA1A2 to 0xA1A6 assigned for arr

[Access] arr[2]
--> Read value at 0xA1A4 = 9
[Output] 9 2. Array Traversal (Summing All Elements)
text
[Start] Sum = 0
[Step] arr[0] = 7 Sum += 7 [Sum: 7]
[Step] arr[1] = 3 Sum += 3 [Sum: 10]
[Step] arr[2] = 9 Sum += 9 [Sum: 19]
[Step] arr[3] = 2 Sum += 2 [Sum: 21]
[Step] arr[4] = 8 Sum += 8 [Sum: 29]
[End] Final Sum: 29 3. Dynamic Array Append
text
[List] []
[Append] 12 => [12]
[Append] 5 => [12, 5]
[Append] 23 => [12, 5, 23]
[Resize] Underlying storage doubled when capacity limit reached (if using dynamic array/list) 4. Insert at Index (Shifting Elements)
text
[Before] arr = [1, 2, 4, 5]
[Insert] 3 at index 2
Move arr[3] (5) to arr[4]
Move arr[2] (4) to arr[3]
Set arr[2] = 3
[After] arr = [1, 2, 3, 4, 5] 5. Remove Duplicates (Sorted Array, Two Pointer)
text
[Start] nums = [1, 1, 2, 2, 3]
[write=1, read=1] nums[read]=1 == nums[write-1]=1 (skip)
[write=1, read=2] nums[read]=2 != nums[write-1]=1 (write nums[write]=2, write++, write=2)
[write=2, read=3] nums[read]=2 == nums[write-1]=2 (skip)
[write=2, read=4] nums[read]=3 != nums[write-1]=2 (write nums[write]=3, write++, write=3)
[Final] First 3 elements: [1,2,3] 6. Sliding Window – Max Sum Subarray of Size 3
text
arr = [2, 1, 5, 1, 3, 2], k = 3

[Init] windowSum = 2+1+5 = 8, maxSum = 8
[Slide] Add arr[3]=1, Remove arr[0]=2, windowSum = 8-2+1 = 7, maxSum = 8
[Slide] Add arr[4]=3, Remove arr[1]=1, windowSum = 7-1+3 = 9, maxSum updated to 9
[Slide] Add arr[5]=2, Remove arr[2]=5, windowSum = 9-5+2 = 6, maxSum = 9
[End] maxSum = 9 7. Prefix Sum Construction (Mutable Array)
text
arr = [4, 5, 7, 3]
prefix[0] = 0
[Step1] prefix[1] = prefix[0] + arr[0] = 0 + 4 = 4
[Step2] prefix[2] = 4 + 5 = 9
[Step3] prefix[3] = 9 + 7 = 16
[Step4] prefix[4] = 16 + 3 = 19

[prefix] = [0, 4, 9, 16, 19]
Query for sum from index 1 to 3: prefix - prefix = 19 - 4 = 15

8. Array Rotation (Right by 2)
   text
   arr = [1, 2, 3, 4, 5], k=2
   Step 1: Reverse whole array → [5, 4, 3, 2, 1]
   Step 2: Reverse first 2 → [4, 5, 3, 2, 1]
   Step 3: Reverse from 2 onward → [4, 5, 1, 2, 3]
   [Output] [4, 5, 1, 2, 3]
9. Kadane’s Algorithm Log (Maximum Subarray)
   text
   arr = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
   Step 0: maxCurr = maxGlobal = -2
   Step 1: maxCurr = max(1, -2+1)=1, maxGlobal=1
   Step 2: maxCurr = max(-3,1-3)=-2, maxGlobal=1
   Step 3: maxCurr = max(4,-2+4)=4, maxGlobal=4
   Step 4: maxCurr = max(-1,4-1)=3, maxGlobal=4
   Step 5: maxCurr = max(2,3+2)=5, maxGlobal=5
   Step 6: maxCurr = max(1,5+1)=6, maxGlobal=6
   Step 7: maxCurr = max(-5,6-5)=1, maxGlobal=6
   Step 8: maxCurr = max(4,1+4)=5, maxGlobal=6
   Result: maxGlobal=6
10. Sparse Array Memory Logging
    text
    [Declare] very large array size 1,000,000
    [Assign] Set arr[2] = 'A'
    [Assign] Set arr[1000007] = 'X'
    [Allocate] Only allocate memory for non-empty indexes (sparse representation)
    [Access] arr[300000] returns null/default, as not set
11. Error Case Log: Out-of-Bounds
    text
    arr = [2,4,6]
    FOR i = 0 TO 3:
    PRINT arr[i]

[Access] arr[2]=6 ✓
[Access] arr[3]=?? → ERROR: index out of range 12. Real System State Example: Real-time Leaderboard Update
text
[Top10Scores] [982, 954, ..., 755]
[NewEvent] Insert 993
[Find position] Insert at index 0
[Shift] Move all lower scores down
[Update] Top10Scores = [993, 982, 954, ..., 755]
[Keep size 10] Drop the last entry if needed
These logs let you "watch the array operate"—element by element, step by step—just as professional engineers and interviewers do when reasoning about code correctness, efficiency, and real-world performance.
