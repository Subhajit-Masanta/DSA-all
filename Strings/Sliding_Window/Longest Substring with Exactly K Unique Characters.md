# Longest Substring with K Uniques

| Topic | Difficulty | Pattern |
|--------|------------|---------|
| Sliding Window | Medium | Variable Sliding Window + HashMap |

---

# 📌 Problem Description

You are given a string `s` consisting only of lowercase alphabets and an integer `k`.

Find the **length of the longest substring** that contains **exactly `k` distinct characters**.

If no such substring exists, return **-1**. :contentReference[oaicite:0]{index=0}

---

# 📘 Examples

### Example 1

```text
Input:

s = "aabacbebebe"

k = 3

Output:

7
```

Explanation:

```text
The longest substring is

"cbebebe"

Distinct Characters:

c, b, e
```

---

### Example 2

```text
Input:

s = "aaaa"

k = 2

Output:

-1
```

Explanation:

```text
Only one distinct character exists.

No valid substring.
```

---

### Example 3

```text
Input:

s = "aabaaab"

k = 2

Output:

7
```

Explanation:

```text
Entire string

"aabaaab"

contains exactly

a,b
```

---

# 🧠 Pattern Recognition

Keywords

```text
Longest

Substring

Exactly K Distinct
```

↓

Think

```text
Variable Sliding Window

+

HashMap (Frequency)
```

---

# 💡 Intuition

Expand the window by moving `right`.

Store frequencies inside

```cpp
unordered_map<char,int>
```

If distinct characters become

```text
> k
```

Shrink from the left.

Whenever

```text
Distinct == k
```

Update the longest length.

---

# 🪜 Algorithm

1.

Expand window

```cpp
mp[s[right]]++;
```

---

2.

If

```text
Distinct > k
```

Shrink.

```cpp
while(mp.size() > k)
```

Remove left character.

If frequency becomes zero,

erase it.

---

3.

Whenever

```text
mp.size()==k
```

Update answer.

---

# Dry Run

```text
s

aabacbebebe

k=3
```

Window

```text
aabac
```

Distinct

```text
a,b,c
```

Valid

Length

```text
5
```

---

Expand

```text
aabacb
```

Still

```text
3 distinct
```

Length

```text
6
```

---

Eventually

```text
cbebebe
```

Distinct

```text
c,b,e
```

Length

```text
7
```

Answer

```text
7
```

---

# ✅ Recommended Code (Cleaner)

```cpp
class Solution {
public:
    int longestKSubstr(string &s, int k) {

        unordered_map<char,int> mp;

        int left = 0;
        int longest = -1;

        for(int right = 0; right < s.size(); right++) {

            mp[s[right]]++;

            while(mp.size() > k) {

                mp[s[left]]--;

                if(mp[s[left]] == 0)
                    mp.erase(s[left]);

                left++;
            }

            if(mp.size() == k) {
                longest = max(longest, right - left + 1);
            }
        }

        return longest;
    }
};
```

---

# ✅ Your Code

```cpp
class Solution {
public:
    int longestKSubstr(string &s, int k) {

        unordered_map<char,int> mp;

        int left = 0;
        int longest = -1;
        int have = 0;

        for(int right = 0; right < s.size(); right++) {

            if(mp.find(s[right]) == mp.end()) {

                have++;

                while(have > k) {

                    mp[s[left]]--;

                    if(mp[s[left]] == 0) {
                        mp.erase(s[left]);
                        have--;
                    }

                    left++;
                }
            }

            mp[s[right]]++;

            if(have == k) {

                int len = right - left + 1;

                longest = max(longest, len);
            }
        }

        return longest;
    }
};
```

---

# ⚠️ Issue in Your Code

The logic works, but it's unnecessarily complicated.

Instead of maintaining

```cpp
have
```

you already have

```cpp
mp.size()
```

which always tells you the number of distinct characters.

So these two lines

```cpp
have++;
```

and

```cpp
have--;
```

are not needed.

The cleaner interview solution simply uses

```cpp
while(mp.size() > k)
```

and

```cpp
if(mp.size() == k)
```

Less code, fewer chances of bugs. :contentReference[oaicite:1]{index=1}

---

# Complexity

### Time

```text
O(n)
```

Each character enters and leaves the window at most once.

---

### Space

```text
O(k)
```

At most `k` distinct characters are stored in the HashMap.

---

# 🎯 Interview Recognition

Question contains

```text
Longest

Substring

Exactly K Distinct
```

↓

Think

```text
Variable Sliding Window

+

HashMap Frequency
```

---

# Template

```cpp
unordered_map<char,int> mp;

left = 0;

for(right...)

{

    mp[s[right]]++;

    while(mp.size() > k)
    {
        mp[s[left]]--;

        if(mp[s[left]] == 0)
            mp.erase(s[left]);

        left++;
    }

    if(mp.size() == k)
        ans = max(ans, right-left+1);
}
```
