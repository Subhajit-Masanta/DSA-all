# Longest Subarray with Sum K (Prefix Sum + HashMap)

| Topic | Difficulty | Pattern |
|--------|------------|---------|
| Prefix Sum | Medium | Prefix Sum + HashMap |

---

# 📌 Problem

Find the **length of the longest subarray** whose sum equals `k`.

Array **may contain negative numbers**, so Sliding Window does **not** work. :contentReference[oaicite:0]{index=0}

---

# 🧠 Pattern Recognition

If you hear:

- Longest Subarray
- Sum = K
- Negative numbers present

Think:

```text
Prefix Sum + HashMap
```

---

# 💡 Core Idea

Maintain a running prefix sum.

```text
prefixSum = sum(0...i)
```

Suppose

```text
Current Prefix = P
```

You want a subarray ending at the current index whose sum is `k`.

Ask:

```text
Which previous prefix should I remove?
```

Answer:

```text
Previous Prefix = P - k
```

If that prefix exists,

```text
Current Prefix - Previous Prefix = k
```

So a valid subarray is found. :contentReference[oaicite:1]{index=1}

---

# 📦 What does the HashMap store?

```text
Key   -> Prefix Sum

Value -> First Index where it appeared
```

Example

```text
Prefix Sum : Index

10 -> 0
15 -> 1
17 -> 2
24 -> 3
```

---

# ❓ Why store the FIRST index?

Example

```text
Prefix Sum 5

appears at

index 2

and again at

index 7
```

For the **longest** subarray,

```text
Current Index - First Index
```

is always larger.

So

```text
Store only the first occurrence.
```

Never overwrite it. :contentReference[oaicite:2]{index=2}

---

# 🔍 Algorithm

For every index

```text
Update Prefix Sum

↓

If Prefix == k

Update answer

↓

Need = Prefix - k

↓

If Need exists in map

Length = CurrentIndex - map[Need]

↓

Update maximum length

↓

Store current prefix
(only if first occurrence)
```

---

# 💻 C++ Code (Optimal)

```cpp
class Solution {
public:
    int longestSubarray(vector<int>& arr, int k) {

        unordered_map<int, int> mp;

        // Prefix sum 0 occurs before the array starts
        mp[0] = -1;

        int prefixSum = 0;
        int maxLen = 0;

        for(int i = 0; i < arr.size(); i++) {

            prefixSum += arr[i];

            // If we've seen (prefixSum - k),
            // then the subarray between them sums to k.
            if(mp.find(prefixSum - k) != mp.end()) {
                maxLen = max(maxLen, i - mp[prefixSum - k]);
            }

            // Store only the first occurrence
            if(mp.find(prefixSum) == mp.end()) {
                mp[prefixSum] = i;
            }
        }

        return maxLen;
    }
};
```

---

# 📌 Code Flow

```text
Initialize

↓

prefixSum = 0

↓

HashMap

0 → -1

↓

For every element

↓

Update Prefix Sum

↓

Need = prefixSum - k

↓

Need exists?

YES

↓

Update maximum length

↓

Store current prefix
(if first occurrence)

↓

Continue
```

---

# ⭐ Why `mp[0] = -1`?

Suppose

```text
arr = [10, 5]

k = 15
```

Current prefix

```text
15
```

Need

```text
15 - 15 = 0
```

Since

```text
0 → -1
```

Length becomes

```text
1 - (-1) = 2
```

which correctly represents the subarray

```text
[10, 5]
```

Without `mp[0] = -1`, you'd need a separate check:

```cpp
if(prefixSum == k)
```

Using `0 → -1` removes that special case and makes the code cleaner. This is the version most commonly used in interviews. :contentReference[oaicite:1]{index=1}



# ⚠️ Common Mistakes

❌ Using Sliding Window (fails with negatives)

❌ Overwriting an existing prefix sum

❌ Storing the latest index instead of the first

❌ Forgetting the case when

```text
Prefix Sum == k
```

---

# ⏱ Complexity

Time

```text
O(n)
```

Space

```text
O(n)
```

---

# 🎯 Interview Trigger

```text
Negative Numbers
        +
Longest Subarray
        +
Target Sum

↓

Prefix Sum + HashMap
```

---

# 📚 Similar Problems

- GFG – Longest Subarray with Sum K
- LeetCode 325 – Maximum Size Subarray Sum Equals K
- LeetCode 560 – Subarray Sum Equals K
- LeetCode 525 – Contiguous Array
- LeetCode 974 – Subarrays Divisible by K

---

# ⭐ Key Difference

## Longest Length Problems

```text
HashMap

Prefix Sum

↓

First Index
```

## Count Problems

```text
HashMap

Prefix Sum

↓

Frequency
```

This single difference separates **LC 325** from **LC 560**. :contentReference[oaicite:3]{index=3}
