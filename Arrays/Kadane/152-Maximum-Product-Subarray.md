
# 152. Maximum Product Subarray

> 🔎 Pattern: Modified Kadane  
> 📂 Topic: Arrays  
> 🏷️ Difficulty: Medium  


## 💡 Key Idea

Track both:
- maxProd
- minProd

Because negative × negative = positive.

---

## 🧠 Why Swap When nums[i] < 0?

Multiplying by negative flips signs.
Previous minProd can become new maxProd.

So swap before updating.

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