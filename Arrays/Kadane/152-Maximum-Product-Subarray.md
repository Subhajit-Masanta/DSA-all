
# 152. Maximum Product Subarray

> 🔎 Pattern: Modified Kadane  
> 📂 Topic: Arrays  
> 🏷️ Difficulty: Medium  


## 📌 Problem
Find contiguous subarray with maximum product.

---

## 💡 Key Idea

Track both:
- maxProd (maximum product ending here)
- minProd (minimum product ending here)

Because:
Negative × Negative = Positive

---

## 🧠 Why Swap When nums[i] < 0 ?

Multiplying by negative flips signs.
Previous minimum can become maximum.
So swap maxProd and minProd.

---

## ⚙️ Algorithm

1. Initialize maxProd, minProd, answer = nums[0]
2. For each element:
   - If negative → swap(maxProd, minProd)
   - Update maxProd
   - Update minProd
   - Update answer

---

## 🧪 Dry Run

`[2,-5,-2,-4,3]`

Correct Answer = 24

---

## ⏱️ Time Complexity
O(n)

## 📦 Space Complexity
O(1)

---

## ⚠️ Common Mistakes
- Only tracking max product.
- Forgetting swap when negative.
- Not handling zero.

---

## 🎯 Interview One-Liner
Track both maximum and minimum product because sign flips matter.

---

## 💻 Code

```cpp
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int maxProd = nums[0];
        int minProd = nums[0];
        int answer = nums[0];

        for (int i = 1; i < nums.size(); i++) {

            if (nums[i] < 0)
                swap(maxProd, minProd);

            maxProd = max(nums[i], maxProd * nums[i]);
            minProd = min(nums[i], minProd * nums[i]);

            answer = max(answer, maxProd);
        }

        return answer;
    }
};