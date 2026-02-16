
# Longest Subarray with Sum K

> 🔎 Pattern: Prefix Sum + HashMap  
> 📂 Topic: Arrays  
> 🏷️ Difficulty: Medium  

---

## 📌 Problem
Find longest subarray with sum equal to k.

---

## 💡 Key Idea

Store:
prefixSum → earliest index

If prefixSum - k exists,
length = i - index

---

## 🧠 Why Store First Occurrence Only?

Earlier index gives longer subarray.
Overwriting reduces length.

---

## ⏱️ Time Complexity
O(n)

## 📦 Space Complexity
O(n)

---

## ⚠️ Common Mistakes

- Overwriting prefixSum index
- Forgetting prefixSum == k case
- Trying sliding window with negative numbers

---

## 🎯 Interview One-Liner
Store earliest prefix sum index to maximize subarray length.

---

## 💻 Code

```cpp
class Solution {
public:
    int longestSubarray(vector<int>& arr, int k) {
        unordered_map<long long, int> mp;
        long long prefixSum = 0;
        int maxLen = 0;

        for (int i = 0; i < arr.size(); i++) {
            prefixSum += arr[i];

            if (prefixSum == k)
                maxLen = i + 1;

            if (mp.find(prefixSum - k) != mp.end())
                maxLen = max(maxLen, i - mp[prefixSum - k]);

            if (mp.find(prefixSum) == mp.end())
                mp[prefixSum] = i;
        }

        return maxLen;
    }
};