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

## 🔥 Key Confusion Cleared

### ❓ Why `count += right - left + 1` ?

Stop memorizing the formula and look at the actual subarrays. 

Every time you expand the window to the `right`, you introduce **new** subarrays. How many? Exactly the length of the current window. 

**Example:**
Imagine your valid window is currently `[A, B, C]`. 
* `left = 0`, `right = 2`. 
* Window length (`right - left + 1`) = **3**.

What are the **new** subarrays that end with the newly added `C`?
1. `[C]`          *(length 1)*
2. `[B, C]`       *(length 2)*
3. `[A, B, C]`    *(length 3)*

That’s exactly **3** new valid subarrays. The formula just mathematically counts the newly formed subarrays every time you move the right pointer.

---

## 🧠 Visualization: EXACT = ATMOST(k) - ATMOST(k-1)

Think of the `AtMost` function as a bouncer at a club.

**Scenario:** We want subarrays with **EXACTLY 2** distinct elements (`k = 2`).
Array: `[1, 2, 1]`

**Step 1: The `AtMost(2)` Bouncer**
* "If your subarray has 1 or 2 distinct elements, you can enter."
* *Who gets in?* * 1-distinct: `[1]`, `[2]`, `[1]`
  * 2-distinct: `[1, 2]`, `[2, 1]`, `[1, 2, 1]`
* *Total Inside = 6*

**Step 2: The `AtMost(1)` Bouncer (The Kicker)**
* "Wait, management says I need to kick out anyone who has 1 distinct element or less."
* *Who gets kicked out?* * `[1]`, `[2]`, `[1]`
* *Total Kicked Out = 3*

**Step 3: The Math**
* Who is left in the club? **Only the 2-distinct subarrays.**
* `[1, 2]`, `[2, 1]`, `[1, 2, 1]`
* 6 (AtMost 2) - 3 (AtMost 1) = **3 (Exactly 2)**

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
