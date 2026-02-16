
# 53. Maximum Subarray

> 🔎 Pattern: Kadane's Algorithm  
> 📂 Topic: Arrays  
> 🏷️ Difficulty: Medium  


## 📌 Problem
Find the contiguous subarray with the largest sum.

---

## 💡 Key Idea

At each index:
- Either extend previous subarray
- Or start fresh from current element

currentSum = max(nums[i], currentSum + nums[i])

---

## 🧠 Why This Works

If currentSum becomes negative:
- It will reduce future sums
- So restart from current element

---

## ⚙️ Algorithm

1. Initialize currentSum and maxSum with nums[0]
2. Traverse array:
   - currentSum = max(nums[i], currentSum + nums[i])
   - maxSum = max(maxSum, currentSum)

---

## 🧪 Dry Run

`[-2,1,-3,4,-1,2,1,-5,4]`

| i | num | currentSum | maxSum |
|---|-----|------------|--------|
|0|-2|-2|-2|
|1|1|1|1|
|2|-3|-2|1|
|3|4|4|4|
|4|-1|3|4|
|5|2|5|5|
|6|1|6|6|

Answer = 6

---

## ⏱️ Time Complexity
O(n)

## 📦 Space Complexity
O(1)

---

## ⚠️ Common Mistakes
- Initializing maxSum to 0.
- Not handling all-negative arrays.

---

## 🎯 Interview One-Liner
Kadane’s algorithm keeps track of best subarray ending at each index.

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