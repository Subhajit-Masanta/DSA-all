# 🔍 Find Minimum in Rotated Sorted Array

| Topic | Difficulty | Pattern |
|---|---|---|
| Binary Search | Medium | Modified Binary Search |

## 📌 Problem

Given a sorted array rotated at some pivot:

```cpp
[0,1,2,4,5,6,7]
```

may become:
```cpp
[4,5,6,7,0,1,2]
```

Return the minimum element.

Must run in:
```cpp
O(log n)
```

---

## 🧠 Core Insight

The array is NOT fully sorted.

BUT:
```cpp
one half is always sorted
```

The minimum element lies at:
# rotation point / break point

Example:
```cpp
[4,5,6,7,0,1,2]
           ^
         minimum
```

---

## 🔥 Key Observation

### If:
```cpp
nums[mid] <= nums[high]
```

Then:
```cpp
[mid ... high]
```

is sorted.

Meaning:
```cpp
minimum may be:
- nums[mid]
- somewhere LEFT
```

So:
```cpp
high = mid
```

---

### Else:
```cpp
nums[mid] > nums[high]
```

Meaning:
```cpp
minimum definitely on RIGHT
```

So:
```cpp
low = mid + 1
```

---

## 🧪 Example

```cpp
nums = [4,5,6,7,0,1,2]
```

### Step 1
```cpp
low = 0
high = 6
mid = 3

nums[mid] = 7
nums[high] = 2
```

Check:
```cpp
7 > 2
```

Meaning:
```cpp
minimum on RIGHT
```

Move:
```cpp
low = mid + 1
```

---

### Step 2
```cpp
low = 4
high = 6
mid = 5

nums[mid] = 1
nums[high] = 2
```

Check:
```cpp
1 <= 2
```

Meaning:
```cpp
minimum may be mid or LEFT
```

Move:
```cpp
high = mid
```

---

Eventually:
```cpp
low == high
```

That index itself is minimum.

---

# ⚠️ Important Understanding

We are NOT searching target.

We are searching:
```cpp
rotation break point
```

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

So use:
```cpp
while(low < high)
```

---

# ⚠️ Common Mistakes

- Using normal binary search logic
- Storing unnecessary `ans`
- Using `high = mid - 1`
- Forgetting minimum may be `mid`
- Infinite loop with `<=`

---

# 💻 Code

```cpp
class Solution {
public:

    int findMin(vector<int>& nums) {

        int low = 0;
        int high = nums.size() - 1;

        while(low < high) {

            int mid = low + (high - low) / 2;

            // Right half sorted
            if(nums[mid] <= nums[high]) {
                high = mid;
            }

            // Minimum on right side
            else {
                low = mid + 1;
            }
        }

        return nums[low];
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
Modified Binary Search

### Core Idea
Find rotation break point

### If
```cpp
nums[mid] <= nums[high]
```
go LEFT

### Else
go RIGHT

### Important
Minimum may be `mid`

### One-liner
Search the unsorted/rotation side to find minimum.
