
# 560. Subarray Sum Equals K

> 🔎 Pattern: Prefix Sum + HashMap  
> 📂 Topic: Arrays  
> 🏷️ Difficulty: Medium  

---

## 📌 Problem
Count number of subarrays whose sum equals k.

---

## 💡 Key Idea

If:
prefixSum - k exists before,
then a valid subarray exists.

Store:
prefixSum → frequency

---

## 🧠 Why Frequency and NOT Index?

Because multiple same prefix sums may exist.
Each occurrence contributes to answer.

---

## ⚙️ Algorithm

1. Initialize prefixSum = 0
2. mp[0] = 1
3. Traverse:
   - prefixSum += nums[i]
   - count += mp[prefixSum - k]
   - mp[prefixSum]++

---

## ⏱️ Time Complexity
O(n)

## 📦 Space Complexity
O(n)

---

## ⚠️ Common Mistakes

- Doing count++ instead of count += frequency
- Storing index instead of frequency
- Forgetting mp[0] = 1

---

## 🎯 Interview One-Liner
Use prefix sum and hashmap storing frequencies to count subarrays.

---

## 💻 Code

```cpp
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        unordered_map<int, int> mp;
        mp[0] = 1;

        int prefixSum = 0;
        int count = 0;

        for (int num : nums) {
            prefixSum += num;

            if (mp.find(prefixSum - k) != mp.end())
                count += mp[prefixSum - k];

            mp[prefixSum]++;
        }

        return count;
    }
};