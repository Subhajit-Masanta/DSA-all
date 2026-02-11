# 167. Two Sum II – Input Array Is Sorted

> 🔎 Pattern: Two Pointers  
> 📂 Topic: Arrays  
> 🏷️ Difficulty: Easy  

---

## 📌 Problem
Given a **sorted** array of integers, return the indices (1-based) of two numbers such that they add up to a target.

---

## 💡 Key Idea

Since the array is sorted:

- If the current sum is **too large**, we must decrease it → move `right` pointer left.
- If the current sum is **too small**, we must increase it → move `left` pointer right.

This works because sorting guarantees:
- Moving left pointer increases sum.
- Moving right pointer decreases sum.

---

## 🧠 Why Two Pointers Work

Sorting gives monotonic behavior:
- Increasing index → value increases.
- Decreasing index → value decreases.

This allows us to eliminate half the search space at every step.

---

## ⚙️ Algorithm

1. Initialize `left = 0`, `right = n - 1`.
2. While `left < right`:
   - Compute `sum = numbers[left] + numbers[right]`
   - If `sum == target` → return `{left + 1, right + 1}`
   - If `sum > target` → `right--`
   - If `sum < target` → `left++`
3. If no pair found, return empty vector.

---

## 🧪 Dry Run

Example:  
`numbers = [2,7,11,15]`, `target = 9`

| left | right | sum | Action |
|------|-------|-----|--------|
| 0(2) | 3(15) | 17  | right-- |
| 0(2) | 2(11) | 13  | right-- |
| 0(2) | 1(7)  | 9   | Found ✔ |

Answer → `{1,2}`

---

## ⏱️ Time Complexity
O(n)  
Each pointer moves at most `n` times.

---

## 📦 Space Complexity
O(1)  
No extra data structures used.

---

## ⚠️ Common Mistakes

- Using HashMap even though array is sorted.
- Forgetting indices must be **1-based**.
- Using `left <= right` instead of `left < right`.
- Forgetting to return empty vector if not found.

---

## 🔁 Pattern Connection

This problem builds foundation for:
- 3Sum
- Container With Most Water
- 4Sum
- Sliding Window problems

---

## 🎯 Interview One-Liner
We exploit the sorted property and use two pointers to adjust the sum in linear time.

---

## 💻 Code

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        int left = 0;
        int right = numbers.size() - 1;

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
