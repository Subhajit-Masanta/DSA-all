# 1749. Maximum Absolute Sum of Any Subarray

| Topic | Difficulty | Pattern |
| :--- | :--- | :--- |
| Arrays | Medium | Dual Kadane (Max & Min Subarray) |

---

## 📌 Problem Description
Given an integer array `nums`, return the **maximum absolute sum** of any (possibly empty) subarray.

**Absolute Value Definition:**
- `abs(x)` is `x` if $x \ge 0$, and `-x` if $x < 0$.

---

## 🧠 Core Insight: The Dual Search
To find the maximum absolute sum, you need to track the two extremes of the array's potential:
1.  **The most positive sum** (Standard Kadane).
2.  **The most negative sum** (Min-Kadane).

The answer is simply:  
$$\max(\text{maxSum}, \text{abs}(\text{minSum}))$$



---

## ⚙️ The Algorithm

1.  Initialize `currentMax`, `maxSum`, `currentMin`, and `minSum` with the first element.
2.  Traverse the array once ($O(n)$):
    -   Update `currentMax` using standard Kadane's.
    -   Update `currentMin` using "Reverse" Kadane's.
3.  After the loop, return the larger of the two magnitudes.

> **Note:** Even if the array contains only negative numbers, the "absolute" part of the problem ensures the result is positive (or 0 if allowed).

---

## 🧪 Example Walkthrough
**Input:** `[2, -5, 1, -4, 3, -2]`

- **Standard Kadane ($maxSum$):** Finds `3` (subarray `[3]`).
- **Min Kadane ($minSum$):** Finds `-8` (subarray `[-5, 1, -4]`).
- **Calculation:** $\max(3, |-8|) = 8$.
- **Result:** `8`

---

## ⏱️ Complexity Analysis
- **Time Complexity:** $O(n)$ — Single linear scan.
- **Space Complexity:** $O(1)$ — Only tracking 4 variables.

---

## ⚠️ Common Mistakes to Avoid
- **Inside-Loop `abs()`:** Do not apply `abs()` to the current sum *during* the Kadane calculation. It breaks the contiguous subarray logic. Only apply it at the very end.
- **Initialization:** If you initialize with `0`, ensure you handle empty vs. non-empty subarray requirements (though for absolute sum, `0` is usually a safe floor).
- **Ignoring the Negative Peak:** Some forget that a very "small" negative number (like -100) has a larger absolute magnitude than a small positive number (like 5).

---

## 💻 Implementation (C++)

```cpp
class Solution {
public:
    int maxAbsoluteSum(vector<int>& nums) {
        int currentMax = 0;
        int maxSum = 0;
        int currentMin = 0;
        int minSum = 0;

        for (int x : nums) {
            // Find max subarray sum
            currentMax = max(x, currentMax + x);
            maxSum = max(maxSum, currentMax);

            // Find min subarray sum
            currentMin = min(x, currentMin + x);
            minSum = min(minSum, currentMin);
        }

        return max(maxSum, -minSum);
    }
};
