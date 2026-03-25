# 992. Subarrays with K Different Integers

| Topic | Difficulty | Pattern |
| :--- | :--- | :--- |
| Arrays | Hard | Sliding Window + HashMap |

---

## 📌 Problem Description

Given an array `nums` and an integer `k`, return the number of **subarrays** that contain **exactly `k` distinct integers**.

---

## 🧠 Core Insight (VERY IMPORTANT)

We **CANNOT** directly count "exactly `k` distinct" using a standard sliding window. 

Instead, we use this mathematical relationship:
> **Exactly k = AtMost(k) - AtMost(k-1)**

---

## 🔥 Why This Works

* **AtMost(k)** → All subarrays with `≤ k` distinct elements.
* **AtMost(k-1)** → All subarrays with `≤ (k-1)` distinct elements.

**Subtracting them:**
Removes all subarrays with `< k` distinct elements, leaving **only** the subarrays with exactly `k` distinct elements.

---

## 🧠 Visualization

Imagine all possible subarrays grouped by their distinct element count:
* 1 distinct
* 2 distinct
* 3 distinct
* ...
* k distinct

`AtMost(k)`   = (1 + 2 + ... + k)
`AtMost(k-1)` = (1 + 2 + ... + k-1)

**Subtract:** Only the `k` distinct group remains.

---

## 🪟 Sliding Window Idea (AtMost)

Maintain a window where:
**distinct elements ≤ k**

Use an `unordered_map<int, int>` to track the frequency of elements in the current window.

---

## ⚙️ Algorithm (AtMost k)

1.  **Initialize:**
    * `left = 0`
    * `count = 0`
    * `map = {}`

2.  **For each `right` pointer:**
    * **Add element:** `mp[nums[right]]++`
    * **While invalid (`mp.size() > k`):**
        * Shrink window: `mp[nums[left]]--`
        * If frequency hits `0` → `mp.erase(nums[left])`
        * Move left pointer: `left++`
    * **Count valid subarrays:** `count += right - left + 1`

---

## 🔥 Key Confusion Cleared

### ❓ Why `count += right - left + 1` ?

Because if the window `[left ... right]` is valid, **all** subarrays ending at `right` within this window are also valid:
* `[right]`
* `[right-1, right]`
* `[right-2, right]`
* ...
* `[left, ..., right]`

The total number of these subarrays is exactly `right - left + 1`.

---

## 🧪 Example

`nums = [1, 2, 1, 2, 3]`, `k = 2`

* **AtMost(2)** = 12  
* **AtMost(1)** = 5  

**Answer:** 12 - 5 = **7**

---

## ⚠️ Important Rules

* Always use `while`, not `if`, to shrink the window.
* **Remove the element** from the map when its frequency becomes `0`.
* The window must always be valid *before* adding to the `count`.

---

## ❌ Common Mistakes

* Trying to write a single sliding window to count "exactly k" directly.
* Not understanding the `right - left + 1` counting logic.
* Forgetting to `erase` the key when the frequency drops to `0`.

---

## 🔥 Key Pattern

> **EXACT = ATMOST(K) - ATMOST(K-1)**

This pattern is also used in problems asking for:
* Exactly `k` odd numbers.
* Exactly `k` replacements.

---

## ⏱ Time Complexity

**O(n)**
The `atMost` function runs in O(n) time. Since we call it twice, it's O(2n), which simplifies to O(n).

---

## 📦 Space Complexity

**O(k)**
The hash map stores at most `k + 1` elements at any given time.

---

## 💻 Code (C++)

```cpp
class Solution {
public:
    int atMost(vector<int>& nums, int k) {
        int left = 0;
        int count = 0;
        unordered_map<int, int> mp;

        for(int right = 0; right < nums.size(); right++) {
            mp[nums[right]]++;

            while(mp.size() > k) {
                mp[nums[left]]--;
                if(mp[nums[left]] == 0) {
                    mp.erase(nums[left]);
                }
                left++;
            }

            // Add all valid subarrays ending at 'right'
            count += right - left + 1;
        }

        return count;
    }

    int subarraysWithKDistinct(vector<int>& nums, int k) {
        return atMost(nums, k) - atMost(nums, k - 1);
    }
};
