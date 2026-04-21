🟢 Pseudocode: Arrays – Universal Patterns, Core Operations, Top Interview Logic

1. Declaration and Basic Access
   text
   DECLARE arr AS ARRAY OF INTEGER SIZE 5
   SET arr[0] = 10
   SET arr[1] = 20
   SET arr[2] = 30
   SET arr[3] = 40
   SET arr[4] = 50

PRINT arr[3] // Output: 40 2. Iteration: Sum of Array Elements
text
DECLARE sum AS INTEGER = 0
FOR i := 0 TO LENGTH(arr) - 1 DO
sum := sum + arr[i]
END FOR
PRINT sum 3. Dynamic Array: Append / Insert
text
DECLARE list AS EMPTY ARRAY

APPEND 5 TO list // [5]
APPEND 12 TO list // [5, 12]
APPEND 7 TO list // [5, 12, 7] 4. Linear Search
text
FUNCTION linearSearch(arr, target):
FOR i := 0 TO LENGTH(arr) - 1 DO
IF arr[i] == target THEN
RETURN i
END IF
END FOR
RETURN -1
END FUNCTION 5. Binary Search (Sorted Array)
text
FUNCTION binarySearch(arr, target):
LEFT := 0
RIGHT := LENGTH(arr) - 1
WHILE LEFT <= RIGHT DO
MID := LEFT + (RIGHT - LEFT) / 2
IF arr[MID] == target THEN
RETURN MID
ELSE IF arr[MID] < target THEN
LEFT := MID + 1
ELSE
RIGHT := MID - 1
END IF
END WHILE
RETURN -1
END FUNCTION 6. Reverse an Array
text
FUNCTION reverse(arr):
LEFT := 0
RIGHT := LENGTH(arr) - 1
WHILE LEFT < RIGHT DO
SWAP arr[LEFT], arr[RIGHT]
LEFT := LEFT + 1
RIGHT := RIGHT - 1
END WHILE
END FUNCTION 7. Two Pointer – Move Zeros to End
text
FUNCTION moveZeros(arr):
WRITE := 0
FOR READ := 0 TO LENGTH(arr)-1 DO
IF arr[READ] != 0 THEN
arr[WRITE] := arr[READ]
WRITE := WRITE + 1
END IF
END FOR
FOR i := WRITE TO LENGTH(arr)-1 DO
arr[i] := 0
END FOR
END FUNCTION 8. Sliding Window – Max Sum Subarray Size K
text
FUNCTION maxSumSubarray(arr, k):
WINDOW_SUM := 0
MAX_SUM := 0
FOR i := 0 TO k-1 DO
WINDOW_SUM := WINDOW_SUM + arr[i]
END FOR
MAX_SUM := WINDOW_SUM
FOR i := k TO LENGTH(arr)-1 DO
WINDOW_SUM := WINDOW_SUM + arr[i] - arr[i-k]
IF WINDOW_SUM > MAX_SUM THEN
MAX_SUM := WINDOW_SUM
END IF
END FOR
RETURN MAX_SUM
END FUNCTION 9. Prefix Sum Construction
text
FUNCTION buildPrefixSum(arr):
PREFIX := ARRAY OF 0 SIZE (LENGTH(arr)+1)
FOR i := 0 TO LENGTH(arr)-1 DO
PREFIX[i+1] := PREFIX[i] + arr[i]
END FOR
RETURN PREFIX
END FUNCTION

// To get sum from index i to j: PREFIX[j+1] - PREFIX[i] 10. Array Rotation (Right by K)
text
FUNCTION rotateArray(arr, k):
k := k MOD LENGTH(arr)
REVERSE(arr, 0, LENGTH(arr)-1)
REVERSE(arr, 0, k-1)
REVERSE(arr, k, LENGTH(arr)-1)
END FUNCTION 11. Kadane’s Algorithm – Max Subarray Sum
text
FUNCTION maxSubArray(arr):
CURR := arr[0]
MAX := arr[0]
FOR i := 1 TO LENGTH(arr)-1 DO
CURR := MAX(arr[i], CURR + arr[i])
MAX := MAX(MAX, CURR)
END FOR
RETURN MAX
END FUNCTION 12. Product of Array Except Self (No Division)
text
FUNCTION productExceptSelf(arr):
N := LENGTH(arr)
LEFT := ARRAY OF 1 SIZE N
RIGHT := ARRAY OF 1 SIZE N
RESULT := ARRAY OF 1 SIZE N

    LEFT[0] := 1
    FOR i := 1 TO N-1 DO
        LEFT[i] := LEFT[i-1] * arr[i-1]
    END FOR

    RIGHT[N-1] := 1
    FOR i := N-2 DOWNTO 0 DO
        RIGHT[i] := RIGHT[i+1] * arr[i+1]
    END FOR

    FOR i := 0 TO N-1 DO
        RESULT[i] := LEFT[i] * RIGHT[i]
    END FOR

    RETURN RESULT

END FUNCTION 13. Merge Intervals (Array of Arrays)
text
FUNCTION mergeIntervals(intervals):
SORT intervals BY start
MERGED := []
FOR interval IN intervals DO
IF MERGED IS EMPTY OR MERGED[-1][1] < interval[0] THEN
APPEND interval TO MERGED
ELSE
MERGED[-1][1] := MAX(MERGED[-1][1], interval[1])
END IF
END FOR
RETURN MERGED
END FUNCTION 14. Error Example: Off-by-One
text
FOR i := 0 TO LENGTH(arr) DO
PRINT arr[i] // ERROR when i == LENGTH(arr)!
END FOR
Always use 0 to LENGTH(arr)-1 for index-based loops!

15. Sparse Array Basic Principle
    text
    SPARSE := {}
    FOR each index IN [desired indexes only] DO
    SPARSE[index] := value
    END FOR
    Access non-set index returns default/null implicitly.
