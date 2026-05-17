# 🔍 Find First and Last Position of Element in Sorted Array

| Topic | Difficulty | Pattern |
| :--- | :--- | :--- |
| Binary Search | Medium | Boundary Binary Search |

---

# 📌 Problem Description

Given a sorted array `nums` and a target value:

Return:
```cpp
[first_occurrence, last_occurrence]
```

If target does not exist:
```cpp
[-1, -1]
```

---

# 🧠 MOST IMPORTANT INSIGHT

This is NOT normal binary search.

We are NOT searching:
```cpp
“Does target exist?”
```

We are searching:
```cpp
“Where is the LEFTMOST target?”
```

and:
```cpp
“Where is the RIGHTMOST target?”
```

These are TWO different goals.

So:
# ✅ Two Binary Searches are needed

---

# 🔥 BIG CONFUSION CLEARED

Students think:
```cpp
greater → left
smaller → right
```

That is incomplete thinking.

Real Binary Search asks:
# “Which side may still contain a BETTER answer?”

---

# 🧠 Core Mental Model

---

# ✅ First Occurrence

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
target may exist more LEFT
```

So:
```cpp
store answer
go LEFT
```

Meaning:
```cpp
high = mid - 1
```

---

# ✅ Last Occurrence

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
target may exist more RIGHT
```

So:
```cpp
store answer
go RIGHT
```

Meaning:
```cpp
low = mid + 1
```

---

# 🔥 VERY IMPORTANT UNDERSTANDING

Binary Search is NOT:
```cpp
compare and move
```

Binary Search is:
# eliminate impossible search space

---

# 🧠 Example

Input:
```cpp
nums = [5,7,7,8,8,10]
target = 8
```

---

# FIRST OCCURRENCE SEARCH

---

## Step 1
```cpp
mid = 2
nums[mid] = 7
```

7 < 8

Target must be on RIGHT:
```cpp
low = mid + 1
```

---

## Step 2
```cpp
mid = 4
nums[mid] = 8
```

Found target.

BUT:
```cpp
maybe earlier 8 exists
```

Store:
```cpp
first = 4
```

Go LEFT:
```cpp
high = mid - 1
```

---

## Step 3
```cpp
mid = 3
nums[mid] = 8
```

Earlier occurrence found.

Store:
```cpp
first = 3
```

Go LEFT again.

Loop ends.

Final:
```cpp
first = 3
```

---

# LAST OCCURRENCE SEARCH

---

## Step 1
```cpp
mid = 2
nums[mid] = 7
```

Go RIGHT.

---

## Step 2
```cpp
mid = 4
nums[mid] = 8
```

Found target.

BUT:
```cpp
maybe later 8 exists
```

Store:
```cpp
last = 4
```

Go RIGHT:
```cpp
low = mid + 1
```

---

## Step 3
```cpp
mid = 5
nums[mid] = 10
```

Too large.

Go LEFT.

Loop ends.

Final:
```cpp
last = 4
```

---

# 🔥 BIG REALIZATION

Direction depends on:
# what answer you want

NOT just comparison.

---

# ⚠️ Common Confusions Cleared

---

# ❓ Why go LEFT when target found in first occurrence?

Suppose:
```cpp
nums = [1,2,2,2,3]
```

and:
```cpp
mid = 2
```

You found:
```cpp
nums[mid] = 2
```

BUT:
```cpp
another 2 exists at index 1
```

So current answer may not be smallest valid index.

Hence:
```cpp
go LEFT
```

---

# ❓ Why go RIGHT in last occurrence?

Because:
```cpp
another target may exist later
```

We want:
```cpp
largest valid index
```

So:
```cpp
go RIGHT
```

---

# ❓ Why must low/high always change?

Because Binary Search requires:
# shrinking search space

Every iteration MUST do:
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

can happen.

---

# ❓ Why doesn't binary search get stuck infinitely?

Because:
```cpp
mid itself gets removed
```

Example:
```cpp
high = mid - 1
```

removes:
```cpp
[mid ... high]
```

Search space strictly shrinks every iteration.

---

# 🔥 Pattern Recognition

This problem belongs to:
# Boundary Binary Search

---

# Related Problems

| Problem | Meaning |
|---|---|
| Lower Bound | first index >= target |
| Upper Bound | first index > target |
| First Occurrence | first index == target |
| Last Occurrence | last index == target |

These are all:
# boundary-search problems

---

# ⚠️ Difference Between Lower Bound and First Occurrence

Students confuse these constantly.

---

# ✅ Lower Bound

Find:
```cpp
first nums[i] >= target
```

Target does NOT need to exist.

Example:
```cpp
nums = [1,3,5]
target = 2
```

Answer:
```cpp
index = 1
```

---

# ✅ First Occurrence

Find:
```cpp
first nums[i] == target
```

Target MUST exist.

Otherwise:
```cpp
-1
```

---

# 🔥 Your Final Code

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

        // Reset search space
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

# ⏱ Complexity

| Type | Complexity |
|---|---|
| Time | O(log n) + O(log n) = O(log n) |
| Space | O(1) |

---

# 📓 Revision Notes

### Pattern:
Boundary Binary Search

### First Occurrence:
Found target → go LEFT

### Last Occurrence:
Found target → go RIGHT

### Core Idea:
Search for better valid answer

### Binary Search Goal:
Shrink impossible region

### One-liner:
Binary Search is about finding transition/boundary points.

---

# 🧠 FINAL MENTAL MODEL

Binary Search is:
# NOT searching values

It is:
# searching boundaries

You repeatedly ask:
``` id="hvt3z9"
Which side cannot contain a better answer?
```

Discard that side.
