# 904. Fruit Into Baskets

| Topic | Difficulty | Pattern |
| :--- | :--- | :--- |
| Arrays | Medium | Sliding Window (Variable Size + HashMap) |

---

## 📌 Problem Description

Given an array `fruits`, where `fruits[i]` represents the type of fruit at tree `i`, you have **2 baskets** and each basket can hold only **one type of fruit**.

You must pick fruits from a **contiguous subarray**, moving left → right.

Return the **maximum number of fruits** you can collect.

---

## 🧠 Core Insight

This problem is functionally equivalent to:
**Find the longest subarray with at most 2 distinct elements.**

---

## 🪟 Sliding Window Idea

Maintain a window `[left ... right]` such that the **number of distinct fruits ≤ 2**.

* **Expand** the window by moving `right`.
* **If invalid** (distinct fruits > 2) → **shrink** from `left`.
* **Always** keep the window valid before calculating the max length.

---

## ⚙️ Algorithm

1.  **Use a hashmap to store frequency:**
    `unordered_map<int, int> mp;`

2.  **Initialize:**
    * `left = 0`
    * `maxLen = 0`

3.  **For each `right` pointer:**
    * Add the current fruit to the map: `mp[fruits[right]]++`
    * **While map size > 2 (Invalid Window):**
        * Decrease the frequency of the left fruit: `mp[fruits[left]]--`
        * If the frequency becomes `0`, **you must erase the key:** `mp.erase(fruits[left])`
        * Move the left pointer: `left++`
    * **Update answer:** `maxLen = max(maxLen, right - left + 1)`

---

## 🧪 Example

**Input:**
`fruits = [1, 2, 3, 2, 2]`

**Process:**
* `[1, 2]` → Valid (2 distinct fruits). length = 2.
* `[1, 2, 3]` → Invalid (3 distinct fruits: 1, 2, 3) → Shrink left → `[2, 3]`.
* Expand → `[2, 3, 2, 2]` → Valid (2 distinct fruits: 2, 3). length = 4.

**Answer:** `4`

---

## ⏱ Time Complexity

**O(n)**
Each element is added to the hashmap and removed at most once.

---

## 📦 Space Complexity

**O(1)** (Technically O(3) which simplifies to O(1))
The hashmap stores a maximum of 3 distinct fruit types at any given time before it is shrunk back to 2.

---

## ⚠️ Common Mistakes

* Using `if` instead of `while` for shrinking the window (a single `if` fails if you somehow ingested multiple invalid elements, though less common in this specific 1-by-1 expansion, `while` is the robust pattern).
* **Forgetting to `erase` the key** when the count becomes 0. If you don't erase it, `mp.size()` will still return > 2, creating an infinite loop or incorrect logic.
* Updating the max length *before* the `while` loop finishes fixing the invalid window.

---

## 🔥 Key Insight

The `while` loop ensures the window becomes valid again. Shrinking the window is entirely dictated by the **size of the hashmap**, not the raw sum of the elements.

---

## 🎯 Interview One-Liner

> "Use a variable-size sliding window with a hashmap to track frequencies, ensuring the map size never exceeds 2, and maximize the window length."

---

## 💻 Code (C++)

```cpp
class Solution {
public:
    int totalFruit(vector<int>& fruits) {
        unordered_map<int, int> mp;
        int left = 0;
        int maxlen = 0;

        for(int right = 0; right < fruits.size(); right++) {
            mp[fruits[right]]++;

            // Shrink window if we have more than 2 types of fruit
            while(mp.size() > 2) {
                mp[fruits[left]]--;
                if(mp[fruits[left]] == 0) {
                    mp.erase(fruits[left]);
                }
                left++;
            }

            maxlen = max(maxlen, right - left + 1);
        }

        return maxlen;
    }
};
