# 239. Sliding Window Maximum

| Topic | Difficulty | Pattern |
| :--- | :--- | :--- |
| Arrays / Deque | Hard | Sliding Window + Monotonic Decreasing Deque |

---

## 📌 Problem Description
Given an array `nums` and a window size `k`, return the maximum value in every sliding window moving from left to right.

**Example:**
`nums = [1, 3, -1, -3, 5, 3, 6, 7]`, `k = 3`
**Output:** `[3, 3, 5, 5, 6, 7]`

---

## 🧠 Core Insight: Why a Monotonic Deque?

**Brute Force:** Checking the max of each window individually takes $O(n \cdot k)$ time. This will cause a Time Limit Exceeded (TLE).

**Optimization:** We need to find the maximum in $O(1)$ time. We achieve this using a **Monotonic Decreasing Deque**.
* It stores **indices**, not values (so we know when an element expires).
* It strictly maintains elements in decreasing order of their values.
* The **front** of the deque always holds the index of the current window's maximum.



---

## ⚙️ The 3 Rules of the Algorithm

For every index `right` entering the window, enforce these rules in order:

1. **The Expired Rule (Pop Front):** If the index at the front of the deque is `<= right - k`, it is no longer in our sliding window.
   *Action:* `while(dq.front() <= right - k) dq.pop_front()`
2. **The Useless Rule (Pop Back):** If the new element `nums[right]` is greater than elements at the back of the deque, those older, smaller elements will *never* be the maximum.
   *Action:* `while(nums[dq.back()] < nums[right]) dq.pop_back()`
3. **The Add & Record Rule (Push & Save):** Push the current index `right` to the back. If the window has reached size `k` (`right >= k - 1`), record the value at the front of the deque as the current window's maximum.

---

## 🧪 Detailed Dry Run
Let's trace `nums = [1, 3, -1, -3, 5]`, `k = 3`.

| `right` | `nums[right]` | Rule 1 (Front Pop) | Rule 2 (Back Pop) | Deque (Indices) | Deque (Values) | `ans` array |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `0` | `1` | - | - | `[0]` | `[1]` | `[]` |
| `1` | `3` | - | `3 > 1` (Pop `0`) | `[1]` | `[3]` | `[]` |
| `2` | `-1` | - | `-1 < 3` (Keep) | `[1, 2]` | `[3, -1]` | `[3]` |
| `3` | `-3` | - | `-3 < -1` (Keep) | `[1, 2, 3]` | `[3, -1, -3]` | `[3, 3]` |
| `4` | `5` | `1 <= 4-3` (**Pop `1`**) | `5 > -3, -1` (**Pop `3, 2`**) | `[4]` | `[5]` | `[3, 3, 5]` |

*Notice at index 4: The old maximum `3` (index 1) expired and was popped from the front. The smaller values `-1` and `-3` were useless against `5` and were popped from the back.*

---

## ⏱ Time & Space Complexity

- **Time Complexity:** $O(n)$
  Every element's index is pushed into the deque exactly once and popped at most once. The `while` loops do not cause nested $O(n)$ behavior.
- **Space Complexity:** $O(k)$
  The deque will hold at most `k` indices at any given point.

---

## ⚠️ Common Mistakes
- **The Segmentation Fault:** Accessing `dq.front()` or `dq.back()` without checking `!dq.empty()` first.
- **Storing Values Instead of Indices:** If you store values, you have no way of knowing if the maximum value at the front belongs to the current window or an old one.
- **Using `if` Instead of `while` for Rule 2:** A new giant element might make multiple older elements useless. You must `while` loop to clear all of them.

---

## 🎯 Interview One-Liner
Maintain a monotonic decreasing deque storing indices, where we pop expired indices from the front and useless smaller indices from the back, ensuring the front always contains the maximum element of the current window.

---

## 💻 Code (C++)

```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int> ans;
        deque<int> dq;

        for (int right = 0; right < nums.size(); right++) {
            
            // 1. Remove elements outside the current window
            while (!dq.empty() && dq.front() <= right - k) {
                dq.pop_front();
            }

            // 2. Remove smaller elements that are useless
            while (!dq.empty() && nums[dq.back()] < nums[right]) {
                dq.pop_back();
            }

            // 3. Insert current element's index
            dq.push_back(right);

            // 4. Record maximum once the first window is fully formed
            if (right >= k - 1) {
                ans.push_back(nums[dq.front()]);
            }
        }

        return ans;
    }
};
