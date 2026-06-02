# Two Pointer Technique

| Pattern | Use Case |
| :--- | :--- |
| Two Pointers | Arrays, Strings, Linked Lists |

---

# 📌 What is Two Pointer?

Two Pointer is a technique where we use **two indices/pointers** instead of one to solve a problem efficiently. The pointers may move:

- Towards each other
- In the same direction
- At different speeds

The goal is usually to reduce a brute-force O(n²) solution to O(n). :contentReference[oaicite:0]{index=0}

---

# 🧠 Core Idea

Instead of checking every possible pair/subarray:

```cpp
for(i=0;i<n;i++)
    for(j=0;j<n;j++)
```

Use two pointers:

```cpp
left
right
```

and move them intelligently.

---

# 🎯 How to Identify Two Pointer Problems

Ask yourself:

### 1. Is the input sorted?

Examples:
- Two Sum II
- Remove Duplicates
- Pair Sum Problems

Strong signal for Two Pointers.

---

### 2. Do I need to find a pair?

Examples:

```plaintext
Find two numbers
Find pair with target sum
Find pair with minimum difference
```

Usually Two Pointers.

---

### 3. Am I dealing with a subarray/window?

Examples:

```plaintext
Longest
Shortest
Maximum
Minimum
At most K
Exactly K
```

Usually Sliding Window (a Two Pointer variant).

---

### 4. Am I comparing both ends?

Examples:

```plaintext
Palindrome
Container With Most Water
Trapping Rain Water
```

Usually opposite-end Two Pointers.

---

# 🔥 Types of Two Pointer Patterns

---

## Type 1: Opposite Ends

Pointers start from both ends.

```cpp
left = 0;
right = n-1;
```

Move inward.

### Used In

- Two Sum II
- 3Sum
- Container With Most Water
- Trapping Rain Water
- Valid Palindrome

### Template

```cpp
while(left < right){

    if(condition)
        return answer;

    else if(need larger value)
        left++;

    else
        right--;
}
```

---

## Type 2: Same Direction (Sliding Window)

Both pointers move left → right.

```cpp
left = 0;

for(right=0; right<n; right++)
```

Window expands and shrinks.

### Used In

- Max Consecutive Ones III
- Fruit Into Baskets
- Minimum Size Subarray Sum
- Subarray Product Less Than K
- Subarrays With K Distinct

### Template

```cpp
for(right){

    add nums[right]

    while(window invalid){
        remove nums[left]
        left++;
    }

    update answer
}
```

---

## Type 3: Fast & Slow Pointer

Pointers move at different speeds.

```cpp
slow += 1
fast += 2
```

### Used In

- Linked List Cycle
- Middle of Linked List
- Happy Number

---

# 📚 Problems We Solved

## Opposite End

### Two Sum II

Key Idea:

```plaintext
Sorted array
sum > target → right--
sum < target → left++
```

---

### 3Sum

Key Idea:

```plaintext
Fix one element
Solve remaining using Two Sum
Skip duplicates
```

---

### Container With Most Water

Key Idea:

```plaintext
Move smaller height
```

Reason:

```plaintext
Area = width × min(height)
```

---

### Sort Colors

Key Idea:

```plaintext
Dutch National Flag Algorithm
```

Pointers:

```plaintext
low
mid
high
```

---

## Sliding Window Variant

### Max Consecutive Ones III

Condition:

```plaintext
zeros <= k
```

---

### Fruit Into Baskets

Condition:

```plaintext
distinct fruits <= 2
```

---

### Minimum Size Subarray Sum

Condition:

```plaintext
sum >= target
```

Goal:

```plaintext
minimum length
```

---

### Subarray Product Less Than K

Condition:

```plaintext
product < k
```

Count:

```plaintext
right - left + 1
```

---

### Subarrays With K Distinct

Key Trick:

```plaintext
Exactly K = AtMost(K) - AtMost(K-1)
```

---

# ⚠️ Common Mistakes

### Mistake 1

Using nested loops immediately.

Think:

```plaintext
Can two pointers reduce O(n²)?
```

---

### Mistake 2

Using `if` instead of `while` in sliding window.

Wrong:

```cpp
if(window invalid)
```

Correct:

```cpp
while(window invalid)
```

---

### Mistake 3

Not understanding window meaning.

Always define:

```plaintext
What does my window represent?
```

---

# ⏱ Complexity

Typical:

```plaintext
Time  : O(n)
Space : O(1)
```

Because each pointer moves at most n times.

---

# 🧠 Mental Model

Two Pointers = Maintain a relationship between two positions.

Questions:

1. Where should left start?
2. Where should right start?
3. When should I move left?
4. When should I move right?
5. What invariant must always remain true?

---

# 🎯 Interview One-Liner

Use two indices that move intelligently to avoid unnecessary comparisons and reduce O(n²) solutions to O(n).
