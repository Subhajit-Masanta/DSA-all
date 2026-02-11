# 167. Two Sum II – Input Array Is Sorted

> 🔎 Pattern: Two Pointers  
> 📂 Topic: Arrays  
> 🏷️ Difficulty: Easy  

---

## 📌 Problem
Given a sorted array, find two numbers such that they add up to target.

---

## 💡 Key Idea
Since the array is sorted:
- If sum is too large → move right pointer left.
- If sum is too small → move left pointer right.
## Algorithm
1. Initialize left = 0, right = n-1.
2. While left < right:
   - If sum == target → return indices.
   - If sum > target → right--
   - If sum < target → left++

## 🧪 Dry Run

Example:  
numbers = [2,7,11,15], target = 9

| left | right | sum | Action |
|------|-------|-----|--------|
| 0(2) | 3(15) | 17  | right-- |
| 0(2) | 2(11) | 13  | right-- |
| 0(2) | 1(7)  | 9   | Found ✔ |

---

## Time Complexity
O(n)

## Space Complexity
O(1)

--- 
## ⚠️ Common Mistakes
- Using hashmap unnecessarily.
- Forgetting 1-based index.
- Using left <= right.

---

## 🎯 Interview One-Liner
We use two pointers from both ends and shrink the window based on sum.
## Code
```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        int left = 0, right = numbers.size() - 1;

        while (left < right) {
            int sum = numbers[left] + numbers[right];
            if (sum == target) {
                return {left + 1, right + 1};
            } 
            else if (sum > target) {
                right--;
            } 
            else {
                left++;
            }
        }
        return {};
    }
};


