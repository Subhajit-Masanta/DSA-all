# Longest Consecutive Subsequence

> 🔎 Pattern: HashSet + Sequence Expansion  
> 📂 Topic: Arrays  
> 🏷️ Difficulty: Medium  

---

## 📌 Problem
Find the length of the longest subsequence of consecutive integers.
Order of elements does not matter.
https://www.geeksforgeeks.org/problems/longest-consecutive-subsequence2449/0
---

## 💡 Key Idea

1. Insert all elements into a HashSet.
2. A number starts a sequence only if `(num - 1)` is NOT present.
3. Expand forward while `(num + 1)` exists.
4. Track maximum length.

This prevents duplicate counting.

---

## 🧠 Why Check (num - 1) NOT Present?

If `(num - 1)` exists, then the sequence already started earlier.
We only start counting from the smallest number of a sequence.

---

## ⚙️ Algorithm

1. Insert all elements into `unordered_set`.
2. For each number:
   - If `(num - 1)` not in set:
       - Count consecutive numbers forward.
3. Return maximum length.

---

## 🧪 Dry Run

Input: `[1,9,3,10,4,20,2]`

Set = {1,2,3,4,9,10,20}

Start at 1 → count 1,2,3,4 → length = 4  
9 → count 9,10 → length = 2  
20 → length = 1  

Answer = 4

---

## ⏱️ Time Complexity
O(n)

Each element is visited at most twice.

---

## 📦 Space Complexity
O(n)

HashSet stores all elements.

---

## ⚠️ Common Mistakes
- Reversing start condition.
- Sorting first (O(n log n)).
- Using nested loops.
- Using unordered_map unnecessarily.

---

## 🎯 Interview One-Liner
Use a HashSet and expand sequences only from numbers whose predecessor does not exist.

---

## 💻 Code

```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& arr) {

        unordered_set<int> st(arr.begin(), arr.end());
        int maxlen = 0;

        for (int num : arr) {

            if (st.find(num - 1) == st.end()) {

                int current = num;
                int count = 1;

                while (st.find(current + 1) != st.end()) {
                    current++;
                    count++;
                }

                maxlen = max(maxlen, count);
            }
        }

        return maxlen;
    }
};
