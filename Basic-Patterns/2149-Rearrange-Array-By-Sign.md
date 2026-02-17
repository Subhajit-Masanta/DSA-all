# 2149. Rearrange Array Elements by Sign

> 🔎 Pattern: Index Placement  
> 📂 Topic: Arrays  
> 🏷️ Difficulty: Easy  

---

## 📌 Problem
Rearrange the array such that:
- Positive and negative numbers alternate.
- Start with a positive number.
- Preserve relative order within same sign.

---

## 💡 Key Idea

Since positives and negatives are equal:

- Even indices → positive
- Odd indices → negative

Use two pointers:
- `posIndex = 0`
- `negIndex = 1`

Place elements directly in result array.

---

## ⚙️ Algorithm

1. Create result array of size `n`.
2. Traverse original array:
   - If positive → place at `posIndex`, increment by 2.
   - If negative → place at `negIndex`, increment by 2.
3. Return result.

---

## ⏱️ Time Complexity
O(n)

## 📦 Space Complexity
O(n)

---

## ⚠️ Common Mistakes
- Not initializing result vector with size `n`.
- Using push_back instead of index assignment.
- Forgetting equal positive/negative guarantee.

---

## 🎯 Interview One-Liner
Use even and odd index pointers to place positives and negatives while preserving order.

---

## 💻 Code

```cpp
class Solution {
public:
    vector<int> rearrangeArray(vector<int>& nums) {
        int n = nums.size();
        vector<int> ans(n);

        int posIndex = 0;
        int negIndex = 1;

        for (int num : nums) {
            if (num > 0) {
                ans[posIndex] = num;
                posIndex += 2;
            } else {
                ans[negIndex] = num;
                negIndex += 2;
            }
        }

        return ans;
    }
};
