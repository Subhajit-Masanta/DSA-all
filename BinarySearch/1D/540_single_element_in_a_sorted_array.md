# 🔍 540. Single Element in a Sorted Array

| Topic | Difficulty | Pattern |
|---|---|---|
| Binary Search | Medium | Binary Search on Pair Pattern |

## 📌 Problem

Given a sorted array where:
- every element appears exactly twice
- except one element appears once

Return the single element.

Must run in:
```cpp
O(log n)
```

and:
```cpp
O(1)
```

space.

---

## 🧠 Core Insight

Before the single element:
```cpp
pairs start at EVEN index
```

Pattern:
```cpp
even odd
```

Example:
```cpp
[1,1,2,3,3,4,4]

 0 1
 2
 3 4
 5 6
```

---

## 🔥 After Single Element

Pair alignment breaks.

Pattern becomes:
```cpp
odd even
```

Because all elements shift by one.

---

# 🧠 Main Idea

We use Binary Search to detect:
# where pair alignment breaks

---

# ⚙️ Algorithm

1. Find `mid`

2. Check:
```cpp
is nums[mid] the single element?
```

3. Else:
- identify pair
- check parity pattern

4. If pair alignment correct:
```cpp
go RIGHT
```

5. Else:
```cpp
go LEFT
```

---

## 🧪 Example

```cpp
nums = [1,1,2,3,3,4,4,8,8]
```

### Step 1
```cpp
mid = 4
nums[mid] = 3
```

Pair:
```cpp
3,3
```

Check parity:
```cpp
(mid-1) % 2
```

Alignment valid.

Meaning:
```cpp
single element on RIGHT
```

---

### Step 2
Eventually:
```cpp
nums[mid]
```

does not match neighbors.

That is answer.

---

# ⚠️ Important Edge Case

Always check:
```cpp
mid == 0
mid == n-1
```

BEFORE:
```cpp
nums[mid-1]
nums[mid+1]
```

Otherwise:
```cpp
out of bounds
```

runtime error occurs.

---

# ⚠️ Common Mistakes

- Accessing `mid-1` when `mid == 0`
- Accessing `mid+1` when `mid == n-1`
- Forgetting pair alignment logic
- Confusing parity checks

---

# 💻 Your Approach Code

```cpp
class Solution {
public:

    int singleNonDuplicate(vector<int>& nums) {

        int low = 0;
        int high = nums.size() - 1;

        while(low <= high) {

            int mid = low + (high - low) / 2;

            // Single element found
            if((mid == 0 || nums[mid] != nums[mid - 1]) &&
               (mid == nums.size() - 1 || nums[mid] != nums[mid + 1])) {

                return nums[mid];
            }

            // Pair with left element
            if(mid > 0 && nums[mid] == nums[mid - 1]) {

                // Left side complete till pair
                if((mid - 1) % 2 == 0) {
                    low = mid + 1;
                }

                else {
                    high = mid - 1;
                }
            }

            // Pair with right element
            else {

                // Left side complete till pair
                if(mid % 2 == 0) {
                    low = mid + 1;
                }

                else {
                    high = mid - 1;
                }
            }
        }

        return -1;
    }
};
```

---

# 🔥 Cleaner Optimal Approach

## 🧠 Core Idea

Force:
```cpp
mid = even index
```

Then:
```cpp
mid and mid+1
```

always form pair positions.

---

## Logic

If:
```cpp
nums[mid] == nums[mid+1]
```

pair alignment correct.

Single element on:
```cpp
RIGHT
```

Else:
```cpp
LEFT including mid
```

---

# 💻 Cleaner Code

```cpp
class Solution {
public:

    int singleNonDuplicate(vector<int>& nums) {

        int low = 0;
        int high = nums.size() - 1;

        while(low < high) {

            int mid = low + (high - low) / 2;

            // Make mid even
            if(mid % 2 == 1) {
                mid--;
            }

            // Pair valid
            if(nums[mid] == nums[mid + 1]) {
                low = mid + 2;
            }

            // Pair broken
            else {
                high = mid;
            }
        }

        return nums[low];
    }
};
```

---

# 🔥 Why Cleaner Approach Better?

- No boundary chaos
- No `mid-1`
- No multiple conditions
- Easier parity handling
- Cleaner binary search pattern

---

# ⏱ Complexity

| Type | Complexity |
|---|---|
| Time | O(log n) |
| Space | O(1) |

---

# 📓 Revision Notes

### Pattern
Binary Search on Pair Alignment

### Before Single
```cpp
even odd
```

### After Single
```cpp
odd even
```

### Core Idea
Find where pair alignment breaks

### One-liner
Single element shifts pair indexing pattern.
