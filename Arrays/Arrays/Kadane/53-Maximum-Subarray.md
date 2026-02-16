---

```md
# 53. Maximum Subarray

> 🔎 Pattern: Kadane’s Algorithm  
> 📂 Topic: Arrays  
> 🏷️ Difficulty: Medium  

---

## 💡 Key Idea

At each index:
- Extend previous subarray
- OR start new from current element

currentSum = max(nums[i], currentSum + nums[i])

---

## 🧠 Insight

If currentSum becomes negative, it hurts future sums.
So we drop it.

---

## ⏱️ Time Complexity
O(n)

## 📦 Space Complexity
O(1)

---

## 💻 Code

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int currentSum = nums[0];
        int maxSum = nums[0];

        for (int i = 1; i < nums.size(); i++) {
            currentSum = max(nums[i], currentSum + nums[i]);
            maxSum = max(maxSum, currentSum);
        }
        return maxSum;
    }
};