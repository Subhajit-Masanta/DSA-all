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

### 1️⃣ Case 1: The "Standard" Subarray (Linear)
The maximum subarray does **not** wrap around.
- **Solution:** Standard Kadane’s Algorithm.

### 2️⃣ Case 2: The "Circular" Subarray (Wrapping)
The maximum subarray wraps around the end. Instead of directly finding it, we find the **minimum** subarray in the middle and subtract it from the total.

> **Formula:** Circular Max = Total Sum − Minimum Subarray

[Image of Maximum Sum Circular Subarray visualization showing the difference between a linear subarray and a wrapping circular subarray]

---

## ⚙️ The Algorithm

1. Compute `totalSum` of the array.
2. Use Kadane’s logic in a single pass to find:
   - `maxSum` (Global Maximum Subarray)
   - `minSum` (Global Minimum Subarray)
3. **The Edge Case:** If all numbers are negative, `totalSum - minSum` results in `0` (an empty subarray), which is invalid. In this case, return `maxSum`.
4. Otherwise, return `max(maxSum, totalSum - minSum)`.

---

## 🧪 Examples

### Example 1: Circular Wins
**Input:** `[5, -3, 5]`
- **Total Sum:** 7  
- **maxSum:** 7  
- **minSum:** -3  
- **Circular Calculation:** $7 - (-3) = 10$  
- **Result:** 10

### Example 2: All Negative Case
**Input:** `[-3, -2, -3]`
- **Total Sum:** -8  
- **maxSum:** -2  
- **minSum:** -8  
- **Circular Calculation:** $-8 - (-8) = 0$ (Invalid)
- **Result:** -2 (maxSum)

---

## ⏱️ Complexity
- **Time Complexity:** $O(n)$ (Single pass)
- **Space Complexity:** $O(1)$ (In-place variables)

---

## ⚠️ Common Mistakes to Avoid
- **The Modulo Trap:** Trying to simulate circularity with `% n` (leads to $O(n^2)$).
- **The Empty Set:** Forgetting that the subarray must be non-empty (the all-negative case).
- **Sum Confusion:** Using `maxSum - minSum` instead of `totalSum - minSum`.

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

            // Standard Kadane's for Max
            currentMax = max(x, currentMax + x);
            maxSum = max(maxSum, currentMax);

            // Modified Kadane's for Min
            currentMin = min(x, currentMin + x);
            minSum = min(minSum, currentMin);
        }
