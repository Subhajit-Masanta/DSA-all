# 75. Sort Colors (Dutch National Flag)

> 🔎 Pattern: Three Pointers  
> 📂 Topic: Arrays  
> 🏷️ Difficulty: Medium  

---

## 📌 Problem
Sort an array containing only 0s, 1s, and 2s in-place.

---

## 💡 Key Idea

Maintain three regions:

[0 ... low-1] → 0s  
[low ... mid-1] → 1s  
[mid ... high] → unknown  
[high+1 ... n-1] → 2s  

---

## 🧠 Why Not Increment mid When nums[mid] == 2?

After swapping with high:
- The element coming to mid is unprocessed.
- We must check it again.

---

## ⚙️ Algorithm

1. Initialize low = 0, mid = 0, high = n-1
2. While mid <= high:
   - If nums[mid] == 0 → swap(low, mid), low++, mid++
   - If nums[mid] == 1 → mid++
   - If nums[mid] == 2 → swap(mid, high), high--

---

## 🧪 Dry Run

`[2,0,2,1,1,0]`

Final → `[0,0,1,1,2,2]`

---

## ⏱️ Time Complexity
O(n)

## 📦 Space Complexity
O(1)

---

## ⚠️ Common Mistakes
- Incrementing mid when nums[mid] == 2
- Forgetting loop condition mid <= high

---

## 🎯 Interview One-Liner
Use three pointers to partition 0s, 1s, and 2s in one pass.

---

## 💻 Code

```cpp
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int low = 0, mid = 0, high = nums.size() - 1;

        while (mid <= high) {
            if (nums[mid] == 0) {
                swap(nums[low], nums[mid]);
                low++;
                mid++;
            }
            else if (nums[mid] == 1) {
                mid++;
            }
            else {
                swap(nums[mid], nums[high]);
                high--;
            }
        }
    }
};
