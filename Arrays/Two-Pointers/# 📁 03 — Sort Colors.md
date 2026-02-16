# 75. Sort Colors (Dutch National Flag)

> 🔎 Pattern: Three Pointers  
> 📂 Topic: Arrays  
> 🏷️ Difficulty: Medium  

---

## 💡 Key Idea

Maintain four regions:

[0 … low-1] → 0s  
[low … mid-1] → 1s  
[mid … high] → unknown  
[high+1 … n-1] → 2s  

---

## ⚙️ Algorithm

If nums[mid] == 0:
- swap(low, mid)
- low++, mid++

If nums[mid] == 1:
- mid++

If nums[mid] == 2:
- swap(mid, high)
- high--
- DO NOT increment mid

---

## 🧠 Why Not Increment mid When nums[mid] == 2?

Because the element swapped from high is unprocessed.
Incrementing mid would skip checking it.

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
    void sortColors(vector<int>& nums) {
        int low = 0, mid = 0, high = nums.size() - 1;

        while (mid <= high) {
            if (nums[mid] == 0) {
                swap(nums[low++], nums[mid++]);
            }
            else if (nums[mid] == 1) {
                mid++;
            }
            else {
                swap(nums[mid], nums[high--]);
            }
        }
    }
};