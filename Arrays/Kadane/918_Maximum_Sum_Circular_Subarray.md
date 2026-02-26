# 918. Maximum Sum Circular Subarray

| Topic | Difficulty | Pattern |
| :--- | :--- | :--- |
| Arrays | Medium | Kadane's Algorithm + Circular Logic |

---

## 📌 Problem Description
Given a **circular integer array** `nums` of length `n`, return the maximum possible sum of a non-empty subarray of `nums`.

**Circular Property:**
- The end of the array connects back to the beginning.
- Each element can be used at most once in a subarray.

---

## 🧠 Core Concept: The Two-Case Logic

The maximum sum can only exist in one of two scenarios:

### 1. The "Standard" Case (Linear)
The maximum subarray does **not** wrap around. It stays within the bounds of the original indices $[0, n-1]$.
- **Solution:** Standard Kadane’s Algorithm.

### 2. The "Circular" Case (Wrapping)
The maximum subarray wraps around the end of the array. This means the **minimum** subarray must be located somewhere in the middle.
- **Solution:** `Total Array Sum - Minimum Subarray Sum`.



---

## ⚙️ The Algorithm

1.  **Calculate Total Sum:** Sum of all elements in the array.
2.  **Find Global Maximum ($maxSum$):** Standard Kadane's to find the best linear subarray.
3.  **Find Global Minimum ($minSum$):** Modified Kadane's to find the worst linear subarray.
4.  **Handle the All-Negative Edge Case:** If all numbers are negative, `maxSum` will be the least negative number, while `totalSum - minSum` would equal 0 (an empty subarray, which is not allowed).

### 🧠 Final Formula
$$
\text{result} = 
\begin{cases} 
maxSum & \text{if } maxSum < 0 \\
\max(maxSum, totalSum - minSum) & \text{otherwise}
\end{cases}
$$

---

## 🧪 Example Walkthrough
**Input:** `[5, -3, 5]`

* **Total Sum:** $5 + (-3) + 5 = 7$
* **Max Subarray (Kadane):** $[5, -3, 5]$ is not contiguous in a linear sense for sum 10, but linear Kadane finds `5` or `5`. Actually, for `[5, -3, 5]`, the $maxSum$ is $7$ if we don't wrap.
* **Min Subarray:** `[-3]`, so $minSum = -3$.
* **Circular Calculation:** $7 - (-3) = 10$.
* **Final Answer:** $\max(7, 10) = 10$.

---

## 💻 Implementation (C++)

```cpp
class Solution {
public:
    int maxSubarraySumCircular(vector<int>& nums) {
        int totalSum = 0;
        int currentMax = 0;
        int maxSum = nums[0];
        int currentMin = 0;
        int minSum = nums[0];

        for (int x : nums) {
            totalSum += x;
            
            // Standard Kadane's (Max)
            currentMax = max(x, currentMax + x);
            maxSum = max(maxSum, currentMax);
            
            // Reverse Kadane's (Min)
            currentMin = min(x, currentMin + x);
            minSum = min(minSum, currentMin);
        }

        // Edge case: If all elements are negative, maxSum is the answer
        if (maxSum < 0) return maxSum;

        return max(maxSum, totalSum - minSum);
    }
};
