# 🔥 Array Pattern Recognizer — DSA Cheat Sheet

> The word **subarray** does NOT automatically mean Sliding Window.  
> Identify the **condition** of the problem first.

---

# 1. Kadane's Algorithm

## Recognize

```text
Maximum / Minimum SUM of a contiguous subarray
Best contiguous sum
Maximum circular subarray sum
Maximum absolute subarray sum
```

## Main Idea

At every element:

```text
CONTINUE previous subarray
        OR
START FRESH from current element
```

```cpp
curr = max(nums[i], curr + nums[i]);
ans = max(ans, curr);
```

## Remember

```text
KADANE = Continue or Restart?
```

### Common Problems

```text
Maximum Subarray
Maximum Circular Subarray
Maximum Absolute Subarray Sum
```

---

# 2. Sliding Window

## Recognize

```text
Contiguous subarray / substring
Fixed size K
Longest / Shortest valid subarray
At most K
No more than K
```

---

## A. Fixed Window

### Recognize

```text
Subarray of SIZE K
```

### Pattern

```cpp
for(int right = 0; right < n; right++) {

    // add nums[right]

    if(right - left + 1 > k) {
        // remove nums[left]
        left++;
    }

    if(right - left + 1 == k) {
        // calculate answer
    }
}
```

### Example

```text
Maximum sum subarray of size K
```

---

## B. Longest Valid Window

### Recognize

```text
Longest subarray with...
At most K...
Maximum length...
```

### Pattern

```cpp
for(int right = 0; right < n; right++) {

    // add right

    while(window is INVALID) {
        // remove left
        left++;
    }

    ans = max(ans, right - left + 1);
}
```

### Example

```text
Max Consecutive Ones III

Valid when:
zeroCount <= k
```

---

## C. Minimum Valid Window

### Recognize

```text
Minimum / Smallest subarray satisfying condition
```

### Pattern

```cpp
for(int right = 0; right < n; right++) {

    // add right

    while(window is VALID) {

        ans = min(ans, right - left + 1);

        // remove left
        left++;
    }
}
```

### Example

```text
Minimum Size Subarray Sum
```

---

## D. Exactly K Trick

```text
Exactly(K)
=
AtMost(K) - AtMost(K - 1)
```

### Example

```text
Subarrays with exactly K distinct integers
```

## Remember

```text
SLIDING WINDOW = Expand Right + Shrink Left
```

---

# 3. Prefix Sum

## Recognize

```text
Range sum queries
Count subarrays with exact sum K
Subarray sum equals K
Need sum from index L to R
```

## Basic Prefix

```cpp
prefix[i] = prefix[i - 1] + nums[i];
```

Range sum:

```text
sum(L to R)
=
prefix[R] - prefix[L - 1]
```

---

## Prefix Sum + HashMap

### Recognize

```text
COUNT subarrays with sum exactly K
```

Main equation:

```text
currentPrefix - oldPrefix = K
```

Therefore:

```text
oldPrefix = currentPrefix - K
```

Ask:

```text
Have I seen (prefix - K) before?
```

## Remember

```text
PREFIX SUM = Use previous cumulative information
```

---

# 4. Two Pointers

## Recognize

```text
Sorted array
Find pair / triplet
Remove duplicates
Move elements in-place
Compare from both ends
```

## Opposite Direction

```text
left →          ← right
```

```cpp
while(left < right) {

    if(condition)
        left++;
    else
        right--;
}
```

### Examples

```text
Two Sum II
3Sum
Container With Most Water
Valid Palindrome
```

---

## Same Direction

```text
slow →
fast ───────→
```

### Examples

```text
Remove Duplicates
Move Zeroes
Remove Element
```

## Remember

```text
SORTED + PAIR/TRIPLET = Think Two Pointers
```

---

# 5. HashMap / HashSet

## Recognize

```text
Frequency
Duplicates
Seen before?
Find complement
Count occurrences
Fast lookup
```

### Examples

```text
Two Sum
Contains Duplicate
Longest Consecutive Sequence
Frequency counting
```

## Remember

```text
Need fast lookup?
→ HashMap / HashSet
```

---

# 6. Binary Search

## A. Normal Binary Search

### Recognize

```text
Sorted array
Find target
First / Last occurrence
Lower bound / Upper bound
```

```text
Search space = array indices
```

---

## B. Binary Search on Answer

### Recognize

```text
Minimum possible maximum
Maximum possible minimum
Can we do this with X?
Find smallest/largest valid answer
```

Pattern:

```text
Choose mid
    ↓
Can we achieve answer with mid?
    ↓
YES / NO
    ↓
Discard half
```

### Examples

```text
Koko Eating Bananas
Ship Packages Within D Days
Allocate Books
Aggressive Cows
```

## Remember

```text
Can I check an answer X?
+
Answers are monotonic?
→ Binary Search on Answer
```

---

# 7. Monotonic Stack

## Recognize

```text
Next Greater Element
Next Smaller Element
Previous Greater / Smaller
Nearest greater / smaller
```

### Pattern

```cpp
while(!st.empty() && condition) {
    st.pop();
}
```

### Examples

```text
Next Greater Element
Daily Temperatures
Largest Rectangle in Histogram
Stock Span
```

## Remember

```text
NEXT / PREVIOUS GREATER / SMALLER
→ Monotonic Stack
```

---

# 8. Monotonic Deque

## Recognize

```text
Maximum / Minimum in every window of size K
```

### Example

```text
Sliding Window Maximum
```

Main idea:

```text
Remove useless elements from back
Remove out-of-window elements from front
Front always stores the answer
```

## Remember

```text
WINDOW MAX/MIN
→ Monotonic Deque
```

---

# 9. Sorting + Intervals

## Recognize

```text
Overlapping intervals
Merge ranges
Meeting times
Minimum arrows / rooms
```

First step:

```cpp
sort(intervals.begin(), intervals.end());
```

Then compare:

```text
current start <= previous end
```

### Examples

```text
Merge Intervals
Insert Interval
Non-overlapping Intervals
Meeting Rooms
```

## Remember

```text
INTERVALS
→ Usually sort first
```

---

# 10. Heap / Priority Queue

## Recognize

```text
Kth largest / smallest
Top K elements
Repeatedly need minimum / maximum
Merge sorted lists
```

### Examples

```text
Kth Largest Element
Top K Frequent Elements
Merge K Sorted Lists
```

## Remember

```text
TOP K / KTH / REPEATED MIN-MAX
→ Heap
```

---

# 11. Dutch National Flag

## Recognize

```text
Array contains only 0, 1, 2
Three-way partition
Sort Colors
```

Pointers:

```text
low  → next position for 0
mid  → scanner
high → next position for 2
```

Rules:

```text
0 → swap with low,  low++, mid++
1 → mid++
2 → swap with high, high--
```

## Remember

```text
0 → LOW
1 → MID moves
2 → HIGH
```

---

# 12. Cyclic Sort

## Recognize

```text
Numbers in range 1 to N
Numbers in range 0 to N
Missing number
Duplicate number
```

Main idea:

```text
Put every number at its correct index
```

For values `1...N`:

```text
correct index = value - 1
```

### Examples

```text
Missing Number
Find All Numbers Disappeared
Find Duplicate
```

---

# ⚠️ Most Confusing Patterns

## Kadane vs Sliding Window vs Prefix Sum

| Question | Pattern |
|---|---|
| Maximum contiguous sum | Kadane |
| Fixed size K | Sliding Window |
| Longest valid subarray | Sliding Window |
| At most K | Sliding Window |
| Count subarrays with exact sum K | Prefix + HashMap |
| Many range sum queries | Prefix Sum |

### Mental Difference

```text
KADANE
→ Continue or Restart?

SLIDING WINDOW
→ Expand or Shrink?

PREFIX SUM
→ Have I seen the required previous sum?
```

---

# ⚠️ Important Negative Number Rule

Normal sum-based Sliding Window often fails with negative numbers.

Why?

```text
Adding a number may decrease the sum
Removing a number may increase the sum
```

So:

```text
Positive / non-negative numbers
→ Sliding Window may work

Negative numbers + exact sum/count
→ Think Prefix Sum + HashMap
```

---

# 🚀 10-Second Array Decision Tree

```text
ARRAY QUESTION
      |
      ├── Best contiguous SUM?
      |       └── KADANE
      |
      ├── Fixed size K?
      |       └── SLIDING WINDOW
      |
      ├── Longest/shortest valid range?
      |       └── SLIDING WINDOW
      |
      ├── Exact subarray sum / count?
      |       └── PREFIX SUM + HASHMAP
      |
      ├── Sorted + pair/triplet?
      |       └── TWO POINTERS
      |
      ├── Frequency / seen before?
      |       └── HASHMAP / SET
      |
      ├── Minimum possible maximum?
      |       └── BINARY SEARCH ON ANSWER
      |
      ├── Next greater/smaller?
      |       └── MONOTONIC STACK
      |
      ├── Window maximum/minimum?
      |       └── MONOTONIC DEQUE
      |
      ├── Top K / Kth element?
      |       └── HEAP
      |
      ├── Overlapping ranges?
      |       └── SORT + INTERVALS
      |
      └── Values from 1 to N?
              └── CYCLIC SORT
```

---

# 🧠 Final Pattern Recognition Table

| Clue in Question | First Pattern to Think |
|---|---|
| Maximum contiguous sum | Kadane |
| Size exactly K | Fixed Sliding Window |
| Longest + at most K | Variable Sliding Window |
| Minimum valid subarray | Shrinking Window |
| Exact sum K + count | Prefix Sum + HashMap |
| Range sum queries | Prefix Sum |
| Sorted + pair | Two Pointers |
| Sorted + search | Binary Search |
| Minimize maximum | Binary Search on Answer |
| Next greater/smaller | Monotonic Stack |
| Maximum in each window | Monotonic Deque |
| Kth / Top K | Heap |
| Frequency / duplicates | HashMap / Set |
| Overlapping intervals | Sort + Merge |
| Only 0, 1, 2 | Dutch National Flag |
| Values 1 to N | Cyclic Sort |

---

# Final Rule

```text
Do NOT identify a pattern from one word like "subarray".

Identify:

1. What is being asked?
   → max / min / count / longest / exact?

2. Is the data contiguous?
   → subarray / substring?

3. Is there a window condition?
   → at most / fixed K / valid?

4. Is exact sum or counting required?
   → prefix sum?

5. Is the array sorted?
   → two pointers / binary search?

Then choose the pattern.
```