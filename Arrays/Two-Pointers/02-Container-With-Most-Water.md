# 11. Container With Most Water

> 🔎 Pattern: Two Pointers  
> 📂 Topic: Arrays  
> 🏷️ Difficulty: Medium  

---

## 📌 Problem
Given an array where each element represents a vertical line, find two lines that form a container holding the maximum water.

---

## 💡 Key Idea

Area formula:

Area = (right - left) × min(height[left], height[right])

To maximize area:
- Start with widest width.
- Always move the pointer pointing to the smaller height.

---

## 🧠 Why Move the Smaller Pointer?

Area is limited by the smaller height.
Moving the taller pointer reduces width but does not increase height.
So only moving the smaller height can potentially increase area.

---

## ⚙️ Algorithm

1. left = 0, right = n-1
2. While left < right:
   - Calculate area
   - Update maxArea
   - Move the smaller height pointer

---

## ⏱️ Time Complexity
O(n)

## 📦 Space Complexity
O(1)

---

## ⚠️ Common Mistakes

- Moving taller pointer.
- Forgetting min(height[i], height[j]).
- Using nested loops (O(n²)).

---

## 💻 Code

```cpp
class Solution {
public:
    int maxArea(vector<int>& height) {
        int left = 0, right = height.size() - 1;
        int maxArea = 0;

        while (left < right) {
            int area = (right - left) * min(height[left], height[right]);
            maxArea = max(maxArea, area);

            if (height[left] < height[right])
                left++;
            else
                right--;
        }
        return maxArea;
    }
};