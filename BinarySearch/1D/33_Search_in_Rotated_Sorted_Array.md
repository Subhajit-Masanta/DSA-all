# 🔍 33. Search in Rotated Sorted Array

| Topic | Difficulty | Pattern |
|---|---|---|
| Binary Search | Medium | Modified Binary Search |

## 📌 Problem

Given a sorted array rotated at some pivot:

```cpp
[0,1,2,4,5,6,7]
```

might become:
```cpp
[4,5,6,7,0,1,2]
```

Given `target`, return its index.
If not found:
```cpp
-1
```

Must run in:
```cpp
O(log n)
```

---

## 🧠 Core Insight

The array is NOT fully sorted.

BUT:
# one half is ALWAYS sorted

At every step:
- either LEFT half sorted
- or RIGHT half sorted

Your job:
```cpp
identify sorted half
```

Then:
```cpp
check if target can exist there
```

If YES:
```cpp
search there
```

Else:
```cpp
discard that half
```

---

## 🔥 Real Binary Search Thinking

We are NOT searching both halves.

We ALWAYS:
```cpp
discard one impossible half
```

That keeps:
```cpp
O(log n)
```

---

# 🧠 How To Detect Sorted Half

## ✅ Left Half Sorted

```cpp
nums[low] <= nums[mid]
```

Meaning:
```cpp
[low ... mid] sorted
```

Now check:
```cpp
nums[low] <= target && target < nums[mid]
```

If TRUE:
```cpp
go LEFT
```

Else:
```cpp
go RIGHT
```

---

## ✅ Right Half Sorted

Else:
```cpp
[mid ... high] sorted
```

Check:
```cpp
nums[mid] < target && target <= nums[high]
```

If TRUE:
```cpp
go RIGHT
```

Else:
```cpp
go LEFT
```

---

## 🧪 Example 1

```cpp
nums = [4,5,6,7,0,1,2]
target = 0
```

### Step 1
```cpp
low = 0
mid = 3
high = 6

nums[mid] = 7
```

Check:
```cpp
nums[low] <= nums[mid]
4 <= 7
```

✅ Left half sorted:
```cpp
[4,5,6,7]
```

Can target exist there?
```cpp
4 <= 0 < 7 ❌
```

So:
```cpp
discard left half
low = mid + 1
```

Now search:
```cpp
[0,1,2]
```

---

## 🧪 Example 2

```cpp
nums = [6,7,0,1,2,3,4,5]
target = 0
```

### Step 1
```cpp
mid = 3
nums[mid] = 1
```

Check:
```cpp
nums[low] <= nums[mid]
6 <= 1 ❌
```

So:
```cpp
RIGHT HALF sorted
```

Sorted half:
```cpp
[1,2,3,4,5]
```

Can target exist there?
```cpp
1 < 0 <= 5 ❌
```

So:
```cpp
discard right half
high = mid - 1
```

---

# ⚠️ Important Confusions Cleared

---

## ❓ Why discard whole half directly?

Because once half is sorted:
```cpp
we can mathematically check
whether target can exist there
```

If impossible:
```cpp
discard entire half immediately
```

Exactly like normal binary search.

---

## ❓ Are we doing binary search inside binary search?

❌ NO.

Only:
```cpp
ONE binary search loop
```

Inside loop:
1. identify sorted half
2. validate target range
3. keep one half
4. discard other

---

## ❓ Why does this still work in O(log n)?

Because every iteration:
```cpp
removes half the search space
```

---

# ⚠️ Common Mistakes

- Searching both halves
- Forgetting one side is always sorted
- Wrong range conditions
- Using `<=` incorrectly
- Not handling tiny arrays

---

# 💻 Code

```cpp
class Solution {
public:

    int search(vector<int>& nums, int target) {

        int low = 0;
        int high = nums.size() - 1;

        while(low <= high) {

            int mid = low + (high - low) / 2;

            if(nums[mid] == target) {
                return mid;
            }

            // Left half sorted
            else if(nums[low] <= nums[mid]) {

                if(nums[low] <= target && target < nums[mid]) {
                    high = mid - 1;
                }
                else {
                    low = mid + 1;
                }
            }

            // Right half sorted
            else {

                if(nums[mid] < target && target <= nums[high]) {
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
One half always sorted

### Left Sorted
```cpp
nums[low] <= nums[mid]
```

### Right Sorted
```cpp
nums[mid] <= nums[high]
```

### Goal
Keep possible half, discard impossible half

### One-liner
Detect sorted half and validate target range.
