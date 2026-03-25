# 704. Binary Search (1D)

| Topic | Difficulty | Pattern |
| :--- | :--- | :--- |
| Arrays | Easy | Binary Search |

---

## 📌 Problem Description

Given a **sorted array** `nums` and a `target` value, return its index if found.
Otherwise, return `-1`.

**Constraint:** Must run in **O(log n)** time.

---

## 🧠 Core Insight

Binary Search *only* works because the array is **sorted**.

At every step:
1.  Compare the target to the middle element.
2.  Eliminate half of the search space based on that comparison.

---

## 🔥 Key Idea

> If `nums[mid] > target` → The answer MUST be on the **left**.
> If `nums[mid] < target` → The answer MUST be on the **right**.

The search space continuously shrinks by half until the target is found or the space is empty.

---

## ⚙️ Algorithm

1.  **Initialize Pointers:**
    * `low = 0`
    * `high = nums.size() - 1`

2.  **While `low <= high` (Valid Search Space):**
    * **Find mid:** `mid = low + (high - low) / 2;`
    * **Match:** If `nums[mid] == target` → return `mid`
    * **Search Left:** If `nums[mid] > target` → `high = mid - 1`
    * **Search Right:** If `nums[mid] < target` → `low = mid + 1`

3.  **Result:** If the loop finishes and nothing was found, return `-1`.

---

## 🧪 Example

**Input:**
`nums = [-1, 0, 3, 5, 9, 12]`, `target = 9`

**Process:**
* `low = 0`, `high = 5` → `mid = 2` (`nums[2] = 3`) → `3 < 9`, so go **right**.
* `low = 3`, `high = 5` → `mid = 4` (`nums[4] = 9`) → **Found!**

**Output:** `4`

---

## ⏱ Complexity

* **Time:** **O(log n)** — The search space is halved at each step.
* **Space:** **O(1)** — Only a few integer variables are used.

---

## ⚠️ Common Mistakes (DO NOT DO THESE)

* **Integer Overflow:** Using `mid = (low + high) / 2`. If `low` and `high` are massive integers, their sum will exceed the 32-bit integer limit before the division happens. **Always use `low + (high - low) / 2`.**
* **Infinite Loops:** Forgetting to do `mid - 1` or `mid + 1` and just setting `low = mid` or `high = mid`.
* **Wrong While Condition:** Using `while (low < high)` instead of `while (low <= high)` and missing the case where the target is the very last element being checked when `low` and `high` point to the exact same index.

---

## 🧠 Pattern Recognition

Use Binary Search when:
* The array is **sorted** (or can be treated as sorted/monotonic).
* You need a drastically fast lookup ($O(\log N)$).
* You can definitively eliminate half the search space based on a single condition.

---

## 💻 Code (C++)

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int low = 0;
        int high = nums.size() - 1;

        while (low <= high) {
            // Prevents integer overflow
            int mid = low + (high - low) / 2;

            if (nums[mid] == target) {
                return mid;
            } else if (nums[mid] > target) {
                high = mid - 1; // Search left half
            } else {
                low = mid + 1;  // Search right half
            }
        }

        return -1;
    }
};
