# 567. Permutation in String

| Topic   | Difficulty | Pattern                                     |
| :------ | :--------- | :------------------------------------------ |
| Strings | Medium     | Fixed Size Sliding Window + Frequency Count |

---

## 📌 Problem Description

Given two strings `s1` and `s2`, return `true` if `s2` contains a permutation of `s1`.

A permutation means the same characters with the same frequencies, but possibly in a different order.

Example:

```text
s1 = "ab"
s2 = "eidbaooo"

Substring "ba" exists in s2.

"ab" and "ba" have the same frequency.

Answer = true
```

---

## 🧠 Core Insight

A permutation is simply an anagram.

So instead of checking every possible permutation:

```text
ab
ba
```

we compare frequencies.

If two strings have identical frequency arrays:

```text
a = 1
b = 1
```

they are permutations of each other.

---

## ⚙️ Algorithm

### Step 1

Build frequency of `s1`.

```cpp
need[26]
```

stores the required frequency.

---

### Step 2

Build frequency of the first window of size:

```cpp
s1.size()
```

inside `s2`.

```cpp
window[26]
```

stores current window frequency.

---

### Step 3

Compare:

```cpp
if(window == need)
```

Return:

```cpp
true
```

---

### Step 4

Slide the window.

For every new position:

#### Remove old character

```cpp
window[s2[left]-'a']--;
left++;
```

#### Add new character

```cpp
window[s2[right]-'a']++;
```

#### Compare frequencies

```cpp
if(window == need)
    return true;
```

---

### Step 5

No matching window found.

Return:

```cpp
false
```

---

## 🧪 Dry Run

### Input

```text
s1 = "ab"
s2 = "eidbaooo"
```

---

### need

```text
a = 1
b = 1
```

---

### First Window

```text
ei
```

Frequency:

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

## ⏱ Time Complexity

Building frequencies:

```text
O(n)
```

Comparing vectors:

```text
O(26)
```

for each window.

Since 26 is constant:

```text
O(n)
```

---

## 📦 Space Complexity

```text
O(26)
```

Which is:

```text
O(1)
```

---

## ⚠️ Common Mistakes

### Mistake 1

Sorting every window.

```text
Too slow.
```

Use frequency counts instead.

---

### Mistake 2

Rebuilding frequency for every window.

```text
O(n*k)
```

Instead:

```text
Remove one character
Add one character
```

while sliding.

---

### Mistake 3

Using variable window.

This is a:

```text
Fixed Size Sliding Window
```

problem.

Window size must always remain:

```cpp
s1.size()
```

---

## 🔥 Key Insight

A permutation and an anagram have the same frequency array.

Therefore:

```cpp
need == window
```

means:

```text
Current window is a permutation of s1.
```

---

## 🎯 Interview One-Liner

Use a fixed-size sliding window and compare frequency arrays of `s1` and the current window in `s2`.

---

## 💻 Code (C++)

```cpp
class Solution {
public:
    bool checkInclusion(string s1, string s2) {

        vector<int> need(26, 0);
        vector<int> window(26, 0);

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

---

## 📚 Pattern Recognition

Keywords:

```text
Permutation
Anagram
Substring
Fixed Length
```

Think:

```text
Fixed Size Sliding Window + Frequency Count
```

---

## 📝 Notebook One-Liner

```text
Build frequency of s1, maintain frequency of a fixed-size window in s2, slide by removing left and adding right, compare frequency arrays.
```
