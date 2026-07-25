# Prefix Sum Cheat Sheet

| Topic | Pattern | Difficulty |
|--------|---------|------------|
| Arrays | Prefix Sum | ⭐ Easy |

---

# 📌 What is Prefix Sum?

A Prefix Sum stores the cumulative sum from the beginning of the array.

```text
prefix[i] = sum of elements from index 0 to i
```

Example

```text
Array

2  4  1  3  6

Prefix

2
6
7
10
16
```

Meaning

```text
prefix[3] = 10

↓

2 + 4 + 1 + 3
```

---

# 💡 Core Idea

Instead of calculating a subarray sum repeatedly,

Calculate cumulative sums **once**.

Then answer queries using subtraction.

```text
Subarray Sum

=

Right Prefix

-

Left Prefix
```

This reduces repeated work from **O(n)** per query to **O(1)** after preprocessing. :contentReference[oaicite:0]{index=0}

---

# 🧠 Mental Model

Think of Prefix Sum as **checkpoints**.

```text
0 -----> i -----> j

prefix[i]

stores

sum(0...i)
```

To get

```text
sum(i...j)
```

remove everything before `i`.

---

# 🔍 When to Think of Prefix Sum?

Whenever you hear:

✅ Range Sum

✅ Subarray Sum

✅ Longest Subarray

✅ Count Subarrays

✅ Many Queries on the Same Array

✅ "Sum from l to r"

Immediately think

```text
Prefix Sum
```

Especially when negative numbers are present, Prefix Sum + HashMap is often the right approach because Sliding Window no longer works reliably. :contentReference[oaicite:1]{index=1}

---

# 📌 Types of Prefix Sum Problems

## 1. Basic Prefix Sum

Goal

```text
Find range sums quickly.
```

Examples

- LC 303
- Range Sum Query

---

## 2. Prefix Sum + HashMap

Goal

```text
Find/count subarrays.
```

Examples

- Longest Subarray with Sum K
- LC 560
- LC 325
- LC 525
- LC 974

---

# 🧩 Recognition Pattern

If the question asks

```text
Subarray
```

Ask yourself

```text
Can I express the answer as

Current Prefix

-

Previous Prefix ?
```

If YES

↓

Think Prefix Sum.

This is the key observation behind most interview prefix-sum problems. :contentReference[oaicite:2]{index=2}

---

# 🚫 Common Mistakes

❌ Recomputing every subarray sum

❌ Using Sliding Window when negatives exist

❌ Forgetting that Prefix Sum is built only once

❌ Overwriting the first occurrence of a prefix sum in longest-length problems

---

# ⭐ Interview Trigger

```text
Range Sum
        ↓
Prefix Sum

Subarray Sum
        ↓
Prefix Sum

Longest/Count Subarray
        ↓
Prefix Sum + HashMap

Many Queries
        ↓
Precompute Prefix
```

---

# 🎯 Problems to Learn in Order

1. LC 303 – Range Sum Query
2. Longest Subarray with Sum K (GFG)
3. LC 325 – Maximum Size Subarray Sum Equals K
4. LC 560 – Subarray Sum Equals K
5. LC 525 – Contiguous Array
6. LC 974 – Subarrays Divisible by K

Master these, and you'll recognize most Prefix Sum interview patterns. :contentReference[oaicite:3]{index=3}
