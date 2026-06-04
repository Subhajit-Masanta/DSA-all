# 567. Permutation in String

| Topic   | Difficulty | Pattern                                     |
| :------ | :--------- | :------------------------------------------ |
| Strings | Medium     | Fixed Size Sliding Window + Frequency Count |

---

## 📌 Problem Description

Given two strings `s1` and `s2`, return `true` if `s2` contains a permutation of `s1`.

A permutation means the same characters with the same frequencies but possibly in a different order.

Example:

```text
s1 = "ab"
s2 = "eidbaooo"

Output: true
```

Because:

```text
"ba"
```

is a permutation of:

```text
"ab"
```

and exists as a substring inside `s2`.

---

## 🧠 Core Insight

We do NOT care about character order.

We only care about:

```text
Character Frequencies
```

Example:

```text
ab
ba
```

Both have:

```text
a = 1
b = 1
```

Therefore they are permutations.

---

## ⚙️ Algorithm

### Step 1

Store frequency of `s1`.

```cpp
vector<int> need(26,0);
```

---

### Step 2

Create frequency of first window in `s2`.

Window size:

```text
s1.length()
```

```cpp
vector<int> window(26,0);
```

---

### Step 3

Compare:

```cpp
need == window
```

If equal:

```cpp
return true;
```

---

### Step 4

Slide the window.

Remove old character:

```cpp
window[s2[left]-'a']--;
```

Move left:

```cpp
left++;
```

Add new character:

```cpp
window[s2[right]-'a']++;
```

Compare again.

---

## 🧪 Dry Run

### Input

```text
s1 = "ab"
s2 = "eidbaooo"
```

---

### Frequency of s1

```text
a = 1
b = 1
```

Need:

```text
[1,1,0,0,0...]
```

---

### First Window

```text
ei
```

Window:

```text
e = 1
i = 1
```

Not equal.

---

### Slide

Remove:

```text
e
```

Add:

```text
d
```

Window:

```text
id
```

Not equal.

---

### Slide

Remove:

```text
i
```

Add:

```text
b
```

Window:

```text
db
```

Not equal.

---

### Slide

Remove:

```text
d
```

Add:

```text
a
```

Window:

```text
ba
```

Frequency:

```text
a = 1
b = 1
```

Matches `need`.

Return:

```text
true
```

---

## 🧠 Why No Sorting?

You might think:

```text
ab -> ab
ba -> ab
```

Sorting works.

But sorting every window is expensive.

Instead:

```text
ab -> [1,1,0,0...]
ba -> [1,1,0,0...]
```

Frequency arrays automatically ignore order.

---

## 🧠 Why Fixed Size Sliding Window?

A permutation of:

```text
s1
```

must have exactly:

```text
s1.length()
```

characters.

Example:

```text
s1 = "abc"
```

Valid window size:

```text
3
```

Only.

Therefore:

```text
Fixed Size Sliding Window
```

is the correct pattern.

---

## 🧠 Pattern Recognition

Keywords:

```text
Permutation
Anagram
Substring
```

Usually indicate:

```text
Frequency Count
+
Fixed Size Sliding Window
```

---

## ⏱ Time Complexity

Building frequencies:

```text
O(n)
```

Comparing:

```cpp
need == window
```

takes:

```text
O(26)
```

which is constant.

Total:

```text
O(n)
```

---

## 📦 Space Complexity

```text
O(26)
```

Two frequency arrays.

Constant space.

---

## ⚠️ Common Mistakes

### Mistake 1

Sorting every window.

```text
Works
But Too Expensive
```

---

### Mistake 2

Rebuilding frequency for every window.

Wrong:

```text
Window 1 -> Build
Window 2 -> Build
Window 3 -> Build
```

Use sliding window instead.

---

### Mistake 3

Forgetting to remove the outgoing character.

When window moves:

```text
abc
```

to:

```text
bcd
```

Remove:

```text
a
```

Add:

```text
d
```

---

### Mistake 4

Using a variable-size window.

This problem always uses:

```text
Window Size = s1.length()
```

---

## 🔥 Key Insight

Two strings are permutations of each other if their frequency arrays are identical.

Therefore:

```cpp
need == window
```

is enough.

---

## 🎯 Interview One-Liner

Maintain a fixed-size sliding window in `s2` and compare its frequency array with `s1`'s frequency array.

---

## 💻 Code (C++)

```cpp
class Solution {
public:
    bool checkInclusion(string s1, string s2) {

        vector<int> need(26,0);
        vector<int> window(26,0);

        if(s1.size() > s2.size()) {
            return false;
        }

        for(char c : s1) {
            need[c - 'a']++;
        }

        for(int i = 0; i < s1.size(); i++) {
            window[s2[i] - 'a']++;
        }

        if(window == need) {
            return true;
        }

        int left = 0;

        for(int right = s1.size(); right < s2.size(); right++) {

            window[s2[left] - 'a']--;
            left++;

            window[s2[right] - 'a']++;

            if(window == need) {
                return true;
            }
        }

        return false;
    }
};
```
