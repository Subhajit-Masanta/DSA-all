# 🔍 162. Find Peak Element

| Topic | Difficulty | Pattern |
|---|---|---|
| Binary Search | Medium | Binary Search on Slope |

## 📌 Problem

A peak element is an element strictly greater than its neighbors.

Given an array `nums`, return the index of any peak element.

You may assume:
```cpp
nums[-1] = nums[n] = -∞
```

Must run in:
```cpp
O(log n)
```

---

## 🧠 Core Insight

This problem is NOT normal binary search.

We are NOT searching:
```cpp
exact target
```

We are searching:
```cpp
slope direction
```

---

# 🔥 Key Observation

At every index:

---

## If:
```cpp
nums[mid] < nums[mid + 1]
```

Meaning:
# ascending slope

Example:
```cpp
1 2 3 4
    ^
```

Peak MUST exist on RIGHT.

Why?
- array increasing
- eventually:
  - it decreases → peak found
  - or boundary becomes peak

So:
```cpp
low = mid + 1
```

---

## Else:
```cpp
nums[mid] > nums[mid + 1]
```

Meaning:
# descending slope

Example:
```cpp
5 4 3
^
```

Peak may be:
- `mid`
- somewhere LEFT

So:
```cpp
high = mid
```

---

# ⚙️ Algorithm

1. Initialize:
```cpp
low = 0
high = n - 1
```

2. While:
```cpp
low < high
```

3. Find:
```cpp
mid
```

4. Compare:
```cpp
nums[mid]
```

with:
```cpp
nums[mid + 1]
```

5. If increasing:
```cpp
go RIGHT
```

6. Else:
```cpp
go LEFT including mid
```

7. When:
```cpp
low == high
```

that index is peak.

---

## 🧪 Example

```cpp
nums = [1,2,3,1]
```

### Step 1
```cpp
low = 0
high = 3
mid = 1
```

Check:
```cpp
2 < 3
```

Ascending slope.

Peak on RIGHT:
```cpp
low = mid + 1
```

---

### Step 2
```cpp
low = 2
high = 3
mid = 2
```

Check:
```cpp
3 > 1
```

Descending slope.

Peak may be:
```cpp
mid itself
```

Move:
```cpp
high = mid
```

Now:
```cpp
low == high == 2
```

Answer:
```cpp
2
```

---

# ⚠️ Important Understanding

We are NOT checking:
```cpp
is mid peak?
```

We are using:
# slope direction

That is the actual binary search pattern.

---

# ⚠️ Why `while(low < high)` ?

Using:
```cpp
high = mid
```

with:
```cpp
while(low <= high)
```

can cause infinite loop.

So:
```cpp
while(low < high)
```

is required.

---

# ⚠️ Common Mistakes

- Returning value instead of index
- Using `while(low <= high)`
- Forgetting `mid + 1`
- Trying to check peak directly
- Confusing slope logic

---

# 💻 Code

```cpp
class Solution {
public:

    int findPeakElement(vector<int>& nums) {

        int low = 0;
        int high = nums.size() - 1;

        while(low < high) {

            int mid = low + (high - low) / 2;

            // Increasing slope
            if(nums[mid] < nums[mid + 1]) {
                low = mid + 1;
            }

            // Decreasing slope
            else {
                high = mid;
            }
        }

        return low;
    }
};
```

---

# ⏱ Complexity

| Type | Complexity |
|---|---|
| Time | O(log n) |
| Space | O(1) |

---

# 📓 Revision Notes

### Pattern
Binary Search on Slope

### Increasing Slope
```cpp
nums[mid] < nums[mid+1]
```
→ go RIGHT

### Decreasing Slope
```cpp
nums[mid] > nums[mid+1]
```
→ go LEFT including mid

### Core Idea
Peak always exists in slope direction

### One-liner
Use slope direction to eliminate half the search space.
