# 438. Find All Anagrams in a String

| Topic   | Difficulty | Pattern                                     |
| :------ | :--------- | :------------------------------------------ |
| Strings | Medium     | Fixed Size Sliding Window + Frequency Count |

---

## 📌 Problem Description

Given two strings:

```cpp
s
p
```

Return all starting indices of `p`'s anagrams in `s`.

Example:

```text
s = "cbaebabacd"
p = "abc"

Output = [0, 6]
```

Because:

```text
cba -> anagram of abc
bac -> anagram of abc
```

---

## 🧠 Core Insight

Anagrams have the same frequency.

Example:

```text
abc
cba
bac
```

All have:

```text
a = 1
b = 1
c = 1
```

Instead of generating permutations, compare frequency arrays.

---

## ⚙️ Algorithm

### Step 1

Build frequency of pattern string.

```cpp
vector<int> need(26,0);
```

Stores frequency of `p`.

---

### Step 2

Build frequency of first window of size:

```cpp
p.size()
```

inside `s`.

```cpp
vector<int> window(26,0);
```

---

### Step 3

Compare frequencies.

```cpp
if(window == need)
```

Store:

```cpp
0
```

because first window starts at index 0.

---

### Step 4

Slide Window

For every new position:

#### Remove Left Character

```cpp
window[s[left]-'a']--;
left++;
```

#### Add Right Character

```cpp
window[s[right]-'a']++;
```

#### Compare Frequencies

```cpp
if(window == need)
```

Store:

```cpp
right - p.size() + 1
```

which is the starting index of current window.

---

### Step 5

Return answer vector.

---

## 🧪 Dry Run

### Input

```text
s = "abab"
p = "ab"
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
ab
```

Frequency matches.

Store:

```text
0
```

Answer:

```text
[0]
```

---

### Slide

Window:

```text
ba
```

Frequency:

```text
a = 1
b = 1
```

Match.

Store:

```text
1
```

Answer:

```text
[0,1]
```

---

### Slide

Window:

```text
ab
```

Match.

Store:

```text
2
```

Answer:

```text
[0,1,2]
```

---

## ⏱ Time Complexity

Building frequencies:

```text
O(n)
```

Comparing frequency arrays:

```text
O(26)
```

per window.

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

Building first window from:

```cpp
p
```

instead of:

```cpp
s
```

Correct:

```cpp
window[s[i]-'a']++;
```

---

### Mistake 2

Forgetting to remove left character while sliding.

Correct:

```cpp
window[s[left]-'a']--;
left++;
```

---

### Mistake 3

Using sorting for every window.

Too slow.

Use frequency arrays.

---

### Mistake 4

Wrong starting index.

Current window start:

```cpp
right - p.size() + 1
```

---

## 🔥 Key Insight

438 and 567 are almost identical.

### 567

```cpp
if(window == need)
    return true;
```

### 438

```cpp
if(window == need)
    ans.push_back(startIndex);
```

Only the action changes.

Pattern remains the same.

---

## 🎯 Interview One-Liner

Maintain frequency of the pattern and a fixed-size sliding window; whenever the frequencies match, store the window's starting index.

---

## 💻 Code (C++)

```cpp
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {

        vector<int> need(26,0);
        vector<int> window(26,0);
        vector<int> ans;

        if(p.size() > s.size()) {
            return {};
        }

        for(char c : p) {
            need[c-'a']++;
        }

        for(int i=0;i<p.size();i++) {
            window[s[i]-'a']++;
        }

        if(window == need) {
            ans.push_back(0);
        }

        int left = 0;

        for(int right = p.size(); right < s.size(); right++) {

            window[s[left]-'a']--;
            left++;

            window[s[right]-'a']++;

            if(window == need) {
                ans.push_back(right + 1 - p.size());
            }
        }

        return ans;
    }
};
```

---

## 📚 Pattern Recognition

Keywords:

```text
Anagram
Permutation
Substring
Fixed Length
All Occurrences
```

Think:

```text
Fixed Size Sliding Window
+
Frequency Count
```

---

## 📝 Notebook One-Liner

```text
Build pattern frequency, maintain frequency of a fixed-size window, slide by removing left and adding right, store start index whenever frequencies match.
```
