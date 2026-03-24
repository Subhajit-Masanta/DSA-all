# 209. Minimum Size Subarray Sum

| Topic | Difficulty | Pattern |
| :--- | :--- | :--- |
| Arrays | Medium | Sliding Window (Variable Size) |

---

## 📌 Problem Description

Given an array of **positive integers** `nums` and an integer `target`, return the **minimum length** of a contiguous subarray whose sum is **greater than or equal to `target`**.

If no such subarray exists, return `0`.

---

## 🧠 Core Insight

All numbers are **positive**, so:
* **Expanding window** → sum increases
* **Shrinking window** → sum decreases

This makes the **sliding window** approach valid.

---

## 🎯 Goal

**Find:** The minimum length subarray such that `sum >= target`.

---

## 🪟 Sliding Window Idea

1.  **Expand** the window to the right until `sum >= target`.
2.  **Shrink** the window from the left to minimize the length.
3.  **Always maintain** a valid window state when shrinking.

---

## ⚙️ Algorithm

1.  **Initialize:**
    * `left = 0`
    * `sum = 0`
    * `minLen = INT_MAX`

2.  **For each `right` from `0 → n-1`:**
    * Add `nums[right]` to `sum`.
    * **While `sum >= target`:**
        * Update answer: `minLen = min(minLen, right - left + 1)`
        * Shrink window: `sum -= nums[left]`
        * Increment left: `left++`

3.  **Result:** If no valid subarray is found (`minLen == INT_MAX`), return `0`.

---

## 🧪 Example

**Input:**
`target = 7`, `nums = [2, 3, 1, 2, 4, 3]`

**Process:**
* `[2, 3, 1, 2]` → `sum = 8` → `length = 4`
* Shrink → `[3, 1, 2]` → `sum = 6` ❌ (Invalid, need to expand)
* Expand → `[3, 1, 2, 4]` → `sum = 10` → shrink → `[4, 3]` → `length = 2` ✔️

**Answer:** `2`

---

## ⏱ Time Complexity

**O(n)**
Each element is added to the sum at most once and removed from the sum at most once.

---

## 📦 Space Complexity

**O(1)**
Only a few primitive integer variables are used.

---

## ⚠️ Common Mistakes

* Using `if` instead of `while` for shrinking the window.
* Updating the minimum length *after* shrinking (doing it in the wrong order).
* Forgetting the edge case when no subarray exists (returning `INT_MAX` instead of `0`).
* Trying to use sliding window when negative numbers exist in the array (this breaks the monotonic property).

---

## 🔥 Key Insight

Unlike many sliding window problems where you try to maximize a subarray, here we **minimize** the window size.

---

## 🎯 Interview One-Liner

> "Expand the window until the sum is greater than or equal to the target, then shrink it from the left as much as possible to find the minimum valid length."

---

## 💻 Code (C++)

```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int left = 0;
        int sum = 0;
        int minLen = INT_MAX;

        for(int right = 0; right < nums.size(); right++) {
            sum += nums[right];

            while(sum >= target) {
                minLen = min(minLen, right - left + 1);
                sum -= nums[left];
                left++;
            }
        }

        return (minLen == INT_MAX) ? 0 : minLen;
    }
};
