# 🔍 Search Insert Position

| Topic | Difficulty | Pattern |
| :--- | :--- | :--- |
| Arrays | Easy | Binary Search |

---

# 📌 Problem Description

Given a sorted array of distinct integers and a target value:

- Return the index if the target exists.
- Otherwise return the index where it should be inserted.

Must run in **O(log n)** time.

---

# 🧠 Core Insight

This problem is basically:

> Find the first position where `target` can fit while maintaining sorted order.

Binary Search helps because the array is sorted.

---

# 🔥 Key Observation

Whenever:
- `nums[mid] < target`
  → target must be on the **right side**

- `nums[mid] > target`
  → target can be inserted at `mid`
  → continue searching left for a better position

---

# ⚙️ Algorithm

1. Initialize:
   - `low = 0`
   - `high = n - 1`

2. While `low <= high`:
   - Find `mid`
   - If target found → return `mid`
   - If target greater → move right
   - Else → store possible answer and move left

3. Return insertion position

---

# 🧪 ALL INSERTION SCENARIOS

---

# ✅ Case 1: Target Exists

Input:
```cpp
nums = [1,3,5,6]
target = 5
```

Process:
``` id="mpzz3u"
mid = 1 → nums[mid] = 3
target > nums[mid]
go right

mid = 2 → nums[mid] = 5
found
```

Output:
```cpp
2
```

---

# ✅ Case 2: Insert Between Elements

Input:
```cpp
nums = [1,3,5,6]
target = 2
```

Process:
``` id="m9g83w"
mid = 1 → nums[mid] = 3

target < nums[mid]
possible insertion index = 1

go left
```

Target should be inserted before `3`.

Output:
```cpp
1
```

---

# ✅ Case 3: Insert At End

Input:
```cpp
nums = [1,3,5,6]
target = 7
```

Process:
``` id="c3m0sv"
mid = 1 → 3
go right

mid = 2 → 5
go right

mid = 3 → 6
go right
```

Now:
```cpp
low = 4
```

Insertion position:
```cpp
4
```

Output:
```cpp
4
```

---

# ✅ Case 4: Insert At Beginning

Input:
```cpp
nums = [1,3,5,6]
target = 0
```

Process:
``` id="98zxul"
mid = 1 → 3
target < 3

possible answer = 1
go left

mid = 0 → 1
target < 1

possible answer = 0
```

Output:
```cpp
0
```

---

# 🔥 Understanding Your Logic

---

# 🟢 When `target > nums[mid]`

Example:
```cpp
target = 7
nums[mid] = 5
```

Meaning:
- Target cannot be on left side
- All left elements are smaller
- Move right

```cpp
low = mid + 1;
```

Possible insertion:
```cpp
mid + 1
```

---

# 🔴 When `target < nums[mid]`

Example:
```cpp
target = 2
nums[mid] = 3
```

Meaning:
- Target can be inserted at `mid`
- But maybe there exists a smaller valid index on left

So:
```cpp
ans = mid;
high = mid - 1;
```

---

# ⚠️ Problem In Your Code

Your code:
```cpp
int ans;
```

This is dangerous.

If array is empty:
```cpp
nums = []
```

`ans` remains uninitialized.

Safer:
```cpp
int ans = nums.size();
```

Why?
Because default insertion at end is always valid.

---

# ⚠️ Common Mistakes

- Forgetting insertion at beginning
- Forgetting insertion at end
- Uninitialized `ans`
- Using linear search instead of binary search

---

# ⏱ Complexity

- Time: `O(log n)`
- Space: `O(1)`

---

# 💻 Code (Improved)

```cpp
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int low = 0;
        int high = nums.size() - 1;

        int ans = nums.size();

        while(low <= high) {
            int mid = low + (high - low) / 2;

            if(nums[mid] == target) {
                return mid;
            }
            else if(nums[mid] < target) {
                low = mid + 1;
            }
            else {
                ans = mid;
                high = mid - 1;
            }
        }

        return ans;
    }
};
```

---

# 🧠 Pattern Recognition

This problem is actually:
> Lower Bound of target

Because:
- We search first position where `nums[i] >= target`

---

# 📓 Revision Notes

### Pattern:
Binary Search / Lower Bound

### Key Idea:
Find first valid insertion position

### Important Logic:
- smaller → go right
- greater → possible answer, go left

### One-liner:
Find first index where element is greater than or equal to target.
