# 76. Minimum Window Substring

| Topic   | Difficulty | Pattern                                      |
| :------ | :--------- | :------------------------------------------- |
| Strings | Hard       | Variable Sliding Window + Frequency Matching |

---

## 📌 Problem Description

Given two strings:

```cpp
s
t
```

Find the smallest substring of `s` that contains all characters of `t` (including duplicates).

If no such window exists, return:

```cpp
""
```

---

## 🧠 Core Insight

Unlike Problems:

```text
567. Permutation in String
438. Find All Anagrams in a String
```

where frequencies must match exactly,

here we only need:

```text
window frequency >= required frequency
```

Example:

```text
t = AABC

Need:
A = 2
B = 1
C = 1
```

Window:

```text
AAABCC
```

Valid because:

```text
A = 3 >= 2
B = 1 >= 1
C = 2 >= 1
```

Extra characters are allowed.

---

## ⚙️ Algorithm

### Step 1

Build required frequencies.

```cpp
vector<int> need(128,0);
```

---

### Step 2

Count unique required characters.

```cpp
required
```

Example:

```text
t = AABC

required = 3

(A,B,C)
```

---

### Step 3

Expand Window

Move:

```cpp
right
```

Add current character.

```cpp
window[ch]++;
```

---

### Step 4

Check Requirement Satisfaction

When:

```cpp
window[ch] == need[ch]
```

one requirement becomes satisfied.

```cpp
formed++;
```

---

### Step 5

Window Valid?

When:

```cpp
formed == required
```

window contains everything needed.

---

### Step 6

Shrink Window

While window remains valid:

```cpp
while(formed == required)
```

#### Update minimum answer

#### Remove left character

```cpp
window[leftChar]--;
```

#### Requirement broken?

```cpp
window[leftChar] < need[leftChar]
```

Then:

```cpp
formed--;
```

Window becomes invalid.

Stop shrinking.

---

### Step 7

Continue expanding.

---

## 🧪 Dry Run

### Input

```text
s = ADOBECODEBANC
t = ABC
```

Need:

```text
A = 1
B = 1
C = 1
```

---

Window grows:

```text
ADOBEC
```

Contains:

```text
A ✓
B ✓
C ✓
```

Valid.

---

Shrink?

Removing:

```text
A
```

breaks requirement.

Stop.

---

Continue expanding.

Eventually:

```text
BANC
```

Contains:

```text
A ✓
B ✓
C ✓
```

Length:

```text
4
```

Smallest valid window.

Answer:

```text
BANC
```

---

## 🧠 formed vs required

### required

Number of unique characters needed.

Example:

```text
t = AABC

required = 3
```

Because:

```text
A
B
C
```

are unique.

---

### formed

Number of requirements currently satisfied.

Example:

```text
Need:
A=2
B=1
C=1
```

Window:

```text
AAB
```

Satisfied:

```text
A ✓
B ✓
C ✗
```

Therefore:

```text
formed = 2
```

---

Window valid when:

```cpp
formed == required
```

---

## ⏱ Time Complexity

```text
O(m + n)
```

Each character enters and leaves the window at most once.

---

## 📦 Space Complexity

```text
O(128)
```

ASCII frequency arrays.

Effectively:

```text
O(1)
```

---

## ⚠️ Common Mistakes

### Mistake 1

Setting:

```cpp
required = t.size();
```

Wrong.

Need:

```text
unique characters only
```

Example:

```text
AABC

required = 3
```

not:

```text
4
```

---

### Mistake 2

Using:

```text
window == need
```

This is NOT an anagram problem.

We only need:

```text
window >= need
```

---

### Mistake 3

Forgetting to shrink.

Finding one valid window is not enough.

Keep shrinking to find the minimum.

---

### Mistake 4

Doing:

```cpp
formed++;
```

every time frequency increases.

Correct:

```cpp
if(window[ch] == need[ch])
    formed++;
```

Only when a requirement becomes satisfied.

---

## 🔥 Key Insight

Expand until valid.

Then:

```text
Shrink while valid.
```

Record the smallest valid window.

This expand → shrink cycle is the foundation of advanced sliding window problems.

---

## 🎯 Interview One-Liner

Use frequency arrays and track how many unique requirements are satisfied; expand until valid, then shrink to find the minimum valid window.

---

## 💻 Code (C++)

```cpp
class Solution {
public:
    string minWindow(string s, string t) {

        vector<int> need(128, 0);
        vector<int> window(128, 0);

        int required = 0;

        for(char c : t) {
            if(need[c] == 0)
                required++;

            need[c]++;
        }

        int formed = 0;

        int left = 0;

        int minLen = INT_MAX;
        int start = 0;

        for(int right = 0; right < s.size(); right++) {

            char ch = s[right];

            window[ch]++;

            if(window[ch] == need[ch])
                formed++;

            while(formed == required) {

                if(right - left + 1 < minLen) {
                    minLen = right - left + 1;
                    start = left;
                }

                char leftChar = s[left];

                window[leftChar]--;

                if(window[leftChar] < need[leftChar])
                    formed--;

                left++;
            }
        }

        if(minLen == INT_MAX)
            return "";

        return s.substr(start, minLen);
    }
};
```

---

## 📚 Pattern Recognition

Keywords:

```text
Minimum
Smallest
Shortest
Contains All Characters
Substring
```

Think:

```text
Variable Sliding Window
+
Frequency Matching
```

---

## 📝 Notebook One-Liner

```text
Expand until all requirements are satisfied, then shrink while valid and keep the smallest window.
```
