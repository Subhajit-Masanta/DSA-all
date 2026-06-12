# 06_Recursive_Binary_Search.md

| Topic                   | Difficulty | Pattern                             |
| :---------------------- | :--------- | :---------------------------------- |
| Recursive Binary Search | Easy       | Divide & Conquer / Linear Recursion |

---

# 📌 Problem Description

Given a **sorted array** and a target value, return its index.

If the target is not present, return `-1`.

---

# 🧠 Core Insight

Binary Search works only on **sorted arrays**.

At every step,

```text
Current Search Space

↓

Find Middle

↓

Compare

↓

Discard Half

↓

Search Remaining Half
```

Since the search space becomes half every time,

```text
Time = O(log n)
```

---

# ⚙️ Algorithm

```text
low > high ?

↓

YES

return -1

---------------------

Find mid

↓

nums[mid] == target ?

↓

YES

return mid

---------------------

nums[mid] > target ?

↓

Search Left Half

(low, mid-1)

---------------------

nums[mid] < target ?

↓

Search Right Half

(mid+1, high)
```

---

# 🌳 Recursion Tree

Example

```text
nums = [1,3,5,7,9,11,13]

target = 9
```

```text
search(0,6)

        |

        +---- search(4,6)

                    |

                    +---- search(4,4)

                                |

                             return 4
```

---

# 🧪 Dry Run

Input

```text
nums = [1,3,5,7,9,11,13]

target = 9
```

---

### Call 1

```text
low = 0

high = 6

mid = 3

nums[mid] = 7
```

```text
7 < 9

↓

Search Right Half
```

Call

```text
solve(4,6)
```

---

### Call 2

```text
low = 4

high = 6

mid = 5

nums[mid] = 11
```

```text
11 > 9

↓

Search Left Half
```

Call

```text
solve(4,4)
```

---

### Call 3

```text
low = 4

high = 4

mid = 4

nums[mid] = 9
```

Found

```text
return 4
```

---

# 💻 Code (C++)

```cpp
class Solution {
public:

    int solve(vector<int>& nums,int target,int low,int high)
    {
        if(low>high)
            return -1;

        int mid=low+(high-low)/2;

        if(nums[mid]==target)
            return mid;

        if(nums[mid]>target)
            return solve(nums,target,low,mid-1);

        return solve(nums,target,mid+1,high);
    }

    int search(vector<int>& nums, int target)
    {
        return solve(nums,target,0,nums.size()-1);
    }
};
```

---

# ⏱ Time Complexity

Every recursive call removes half of the search space.

```text
n

↓

n/2

↓

n/4

↓

n/8

↓

...

↓

1
```

Therefore

```text
O(log n)
```

---

# 📦 Space Complexity

Maximum recursion depth

```text
log n
```

Therefore

```text
O(log n)
```

---

# ⚠️ Common Mistakes

### ❌ Wrong Update

```cpp
high = high - mid;
```

Correct

```cpp
high = mid - 1;
```

---

### ❌ Wrong Update

```cpp
low = low + mid;
```

Correct

```cpp
low = mid + 1;
```

---

### ❌ Passing mid as Parameter

```cpp
solve(nums,target,mid,low,high);
```

Not required.

Always compute

```cpp
int mid = low + (high-low)/2;
```

inside the recursive function.

---

### ❌ Wrong Base Case

```cpp
if(low >= high)
```

Correct

```cpp
if(low > high)
    return -1;
```

because `low == high` still represents one valid element.

---

# 🔥 Key Insight

```text
Sorted Array

↓

Find Middle

↓

Target Found ?

↓

YES → Return Index

↓

NO

↓

Discard Half

↓

Recursive Search
```

---

# 🎯 Interview One-Liner

> **Recursive Binary Search is a Divide & Conquer algorithm that repeatedly halves the search space, achieving O(log n) time complexity.**

---

# 📚 Pattern Recognition

```text
Problem

↓

Can half of the search space be discarded?

↓

YES

↓

Divide & Conquer

↓

Recursive Binary Search
```

### Similar Problems

```text
✅ Binary Search

✅ Search Insert Position

✅ First Bad Version

✅ Guess Number Higher or Lower

✅ Peak Element

✅ Search in Rotated Sorted Array
```
