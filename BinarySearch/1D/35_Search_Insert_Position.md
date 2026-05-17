# 🔍 35. Search Insert Position

| Topic | Difficulty | Pattern |
| :--- | :--- | :--- |
| Binary Search | Easy | Lower Bound |

---

## 📌 Problem Description

Given a sorted array of distinct integers `nums` and a target value `target`:

- Return the index if target exists
- Otherwise return the index where it should be inserted

Constraint:
- Must run in `O(log n)`

---

## 🧠 Core Insight

This is NOT just normal binary search.

This problem is actually asking for:

> Find the first position where value >= target

This is the **Lower Bound** pattern.

---

## 🔥 Key Observation

If target is not found:

- insertion position = first element greater than target
- OR position where target should exist

At the end of binary search:

```cpp
low == insertion position
```

So we can directly return `low`.

---

## ⚙️ Algorithm

1. Initialize:
   - `low = 0`
   - `high = n - 1`

2. While `low <= high`:
   - Find mid
   - If `nums[mid] < target`
     - search right
   - Else
     - search left

3. Return `low`

---

## 🧪 Example

Input:
```cpp
nums = [1,3,5,6]
target = 2
```

Steps:

| low | high | mid | nums[mid] | Action |
|---|---|---|---|---|
| 0 | 3 | 1 | 3 | move left |
| 0 | 0 | 0 | 1 | move right |

Loop ends:
```cpp
low = 1
```

Insert position = `1`

---

## 🔥 Mental Model

Binary Search is not searching for value.

It is searching for:

> First valid position

Here:
```cpp
nums[mid] >= target
```

is the valid condition.

---

## ⚠️ Review of Your Code

Your code works.

But your thinking is weak here:

```cpp
int ans;
```

You are manually maintaining answer updates.

Unnecessary.

This problem becomes MUCH cleaner if you understand:

```cpp
return low;
```

at the end.

---

## ❌ Common Mistakes

### 1. Maintaining extra `ans`
Adds unnecessary complexity.

---

### 2. Forgetting insertion can happen at end
Example:
```cpp
nums = [1,3,5]
target = 7
```

Answer:
```cpp
3
```

---

### 3. Thinking this is exact-match binary search
This is actually:
> Lower Bound

Very important distinction.

---

## 🧠 Pattern Recognition

This pattern is used in:

- Lower Bound
- Upper Bound
- First Occurrence
- Last Occurrence
- Search Insert Position

---

## ⏱ Complexity

- Time: `O(log n)`
- Space: `O(1)`

---

## 💻 Cleaner Code (C++)

```cpp
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {

        int low = 0;
        int high = nums.size() - 1;

        while(low <= high) {

            int mid = low + (high - low) / 2;

            if(nums[mid] < target) {
                low = mid + 1;
            }
            else {
                high = mid - 1;
            }
        }

        return low;
    }
};
```

---

## 🧠 One-Liner

> Search for the first position where value becomes greater than or equal to target.

---

## 📓 Notebook Notes

### Pattern:
Lower Bound Binary Search

### Key Idea:
Find first index where:
```cpp
nums[i] >= target
```

### Trick:
After loop:
```cpp
low = insertion position
```

### One-liner:
Binary search on valid position, not exact value.

---

## 🔥 Interview-Level Understanding Check

Answer these before moving on:

1. Why does returning `low` always work?
2. Why do we move:
```cpp
high = mid - 1
```
even when target is found?
3. What happens if array contains duplicates?

If you cannot answer these clearly,
you do NOT understand Lower Bound yet.
