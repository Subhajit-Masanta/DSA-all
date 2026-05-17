# 🔍 34. Find First and Last Position of Element in Sorted Array

| Topic | Difficulty | Pattern |
|---|---|---|
| Binary Search | Medium | Boundary Binary Search |

## 📌 Problem
Given a sorted array `nums` and a `target`, return:
```cpp
[first_occurrence, last_occurrence]
```

If target does not exist:
```cpp
[-1, -1]
```

Must run in:
```cpp
O(log n)
```

### Examples
```cpp
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
```

```cpp
Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
```

```cpp
Input: nums = [], target = 0
Output: [-1,-1]
```

---

## 🧠 Core Insight

This is NOT normal binary search.

We are NOT searching:
```cpp
"Does target exist?"
```

We are searching:
```cpp
"Where is the LEFTMOST target?"
```

and:
```cpp
"Where is the RIGHTMOST target?"
```

So:
```cpp
Two Binary Searches are needed
```

---

## 🔥 Real Binary Search Thinking

Wrong thinking:
```cpp
greater → left
smaller → right
```

Correct thinking:
```cpp
Which side may still contain a BETTER answer?
```

Binary Search = eliminating impossible region.

---

## ✅ First Occurrence

Goal:
```cpp
smallest valid index
```

When:
```cpp
nums[mid] == target
```

Do NOT stop.

Because:
```cpp
another target may exist on LEFT
```

So:
```cpp
first = mid
high = mid - 1
```

---

## ✅ Last Occurrence

Goal:
```cpp
largest valid index
```

When:
```cpp
nums[mid] == target
```

Do NOT stop.

Because:
```cpp
another target may exist on RIGHT
```

So:
```cpp
last = mid
low = mid + 1
```

---

## 🧪 Example

```cpp
nums = [5,7,7,8,8,10]
target = 8
```

### First Occurrence
```cpp
mid = 4 → nums[mid] = 8
store answer
go LEFT

mid = 3 → nums[mid] = 8
store better answer
go LEFT
```

Result:
```cpp
first = 3
```

### Last Occurrence
```cpp
mid = 4 → nums[mid] = 8
store answer
go RIGHT
```

Result:
```cpp
last = 4
```

---

## ⚠️ Common Confusions

### ❓ Why go LEFT after finding target?

Because:
```cpp
smaller valid index may exist
```

Example:
```cpp
[1,2,2,2,3]
```

Finding:
```cpp
mid = 2
```

does NOT guarantee first occurrence.

---

### ❓ Why go RIGHT for last occurrence?

Because:
```cpp
larger valid index may exist
```

---

### ❓ Why must low/high always update?

Binary Search requires:
```cpp
shrinking search space
```

Every iteration must do:
```cpp
low = mid + 1
```

OR:
```cpp
high = mid - 1
```

Otherwise:
```cpp
infinite loop
```

---

## 🔥 Pattern Connection

| Problem | Meaning |
|---|---|
| Lower Bound | first index >= target |
| Upper Bound | first index > target |
| First Occurrence | first index == target |
| Last Occurrence | last index == target |

These are all:
```cpp
Boundary Binary Search
```

---

## ⚠️ Lower Bound vs First Occurrence

### Lower Bound
Find:
```cpp
first nums[i] >= target
```

Target may NOT exist.

Example:
```cpp
nums = [1,3,5]
target = 2

answer = 1
```

---

### First Occurrence
Find:
```cpp
first nums[i] == target
```

If target absent:
```cpp
-1
```

---

## 💻 Code

```cpp
class Solution {
public:

    vector<int> searchRange(vector<int>& nums, int target) {

        int low = 0;
        int high = nums.size() - 1;

        int first = -1;
        int last = -1;

        // First Occurrence
        while(low <= high) {

            int mid = low + (high - low) / 2;

            if(nums[mid] > target) {
                high = mid - 1;
            }

            else if(nums[mid] < target) {
                low = mid + 1;
            }

            else {
                first = mid;
                high = mid - 1;
            }
        }

        low = 0;
        high = nums.size() - 1;

        // Last Occurrence
        while(low <= high) {

            int mid = low + (high - low) / 2;

            if(nums[mid] > target) {
                high = mid - 1;
            }

            else if(nums[mid] < target) {
                low = mid + 1;
            }

            else {
                last = mid;
                low = mid + 1;
            }
        }

        return {first, last};
    }
};
```

---

## ⏱ Complexity

| Type | Complexity |
|---|---|
| Time | O(log n) |
| Space | O(1) |

---

## 📓 Revision Notes

### Pattern
Boundary Binary Search

### First Occurrence
Found target → go LEFT

### Last Occurrence
Found target → go RIGHT

### Core Idea
Search for better valid answer

### One-liner
Binary Search is about finding boundaries, not just values.
