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
