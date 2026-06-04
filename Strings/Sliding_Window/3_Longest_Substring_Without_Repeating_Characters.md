# 3. Longest Substring Without Repeating Characters

| Topic   | Difficulty | Pattern                   |
| :------ | :--------- | :------------------------ |
| Strings | Medium     | Sliding Window + Hash Set |

---

## 📌 Problem Description

Given a string `s`, return the length of the longest substring without repeating characters.

A substring must be contiguous.

Example:

```text
Input:  "abcabcbb"
Output: 3

Explanation:
"abc" is the longest substring without duplicates.
```

---

## 🧠 Core Insight

We need a window that always contains unique characters.

Whenever a duplicate appears:

```text
Expand → Invalid Window
```

we must:

```text
Shrink from Left
```

until the duplicate disappears.

This is a classic:

```text
Sliding Window
```

problem.

---

## ⚙️ Algorithm

Maintain:

```cpp
left
right
unordered_set<char> st
```

The set stores all characters currently inside the window.

---

### Expand

Move:

```cpp
right
```

forward.

---

### Duplicate Found

If:

```cpp
st.find(s[right]) != st.end()
```

the window becomes invalid.

Shrink:

```cpp
while(duplicate exists)
{
    remove s[left]
    left++
}
```

---

### Insert New Character

After the duplicate is removed:

```cpp
st.insert(s[right]);
```

---

### Update Answer

```cpp
length = right - left + 1;
```

Store maximum length.

---

## 🧪 Dry Run

Input:

```text
pwwkew
```

---

### right = 0

Window:

```text
p
```

Set:

```text
{p}
```

Length:

```text
1
```

---

### right = 1

Window:

```text
pw
```

Set:

```text
{p,w}
```

Length:

```text
2
```

---

### right = 2

New character:

```text
w
```

Duplicate found.

Current:

```text
window = pw
set    = {p,w}
```

---

Remove:

```text
p
```

Set:

```text
{w}
```

Still duplicate.

---

Remove:

```text
w
```

Set:

```text
{}
```

Duplicate gone.

---

Insert new:

```text
w
```

Set:

```text
{w}
```

Window becomes valid again.

---

Continue.

Final Answer:

```text
3
```

Substring:

```text
wke
```

---

## 🧠 Understanding Left and Right

### Right Pointer

Brings characters INTO the window.

Therefore:

```cpp
st.insert(s[right]);
```

---

### Left Pointer

Removes characters FROM the window.

Therefore:

```cpp
st.erase(s[left]);
```

---

### Golden Rule

```text
right = entering

left = leaving
```

Remember this forever.

Almost every sliding window problem follows this idea.

---

## 🧠 Why Remove From Set?

The set must always represent the current window.

Example:

```text
window = abc
set    = {a,b,c}
```

If left moves past:

```text
a
```

then:

```text
a
```

is no longer inside the window.

Therefore:

```cpp
st.erase('a');
```

must happen.

Otherwise:

```text
Set ≠ Window
```

and the logic breaks.

---

## 🧠 STL Notes Used

### unordered_set

Stores only values.

Example:

```cpp
unordered_set<char> st;
```

---

Insert:

```cpp
st.insert('a');
```

---

Find:

```cpp
st.find('a') != st.end()
```

Meaning:

```text
Does 'a' exist?
```

---

Erase:

```cpp
st.erase('a');
```

---

### unordered_set vs unordered_map

#### unordered_set

Stores:

```text
Value
```

Example:

```text
a
b
c
```

Used when we only need:

```text
Exists?
Present?
Unique?
```

---

#### unordered_map

Stores:

```text
Key -> Value
```

Example:

```text
a -> 3
b -> 7
```

Used for:

```text
Frequency
Mapping
Index Storage
```

---

## ⏱ Time Complexity

```text
O(n)
```

Each character enters and leaves the window at most once.

---

## 📦 Space Complexity

```text
O(k)
```

Where:

```text
k = number of unique characters
```

Worst case:

```text
O(n)
```

---

## ⚠️ Common Mistakes

### Mistake 1

Only moving:

```cpp
left++;
```

without:

```cpp
st.erase(s[left]);
```

Then the set no longer matches the window.

---

### Mistake 2

Using:

```cpp
if(...)
```

instead of:

```cpp
while(...)
```

Multiple duplicates may need to be removed.

---

### Mistake 3

Forgetting to insert:

```cpp
s[right]
```

after shrinking.

The entire shrinking process is done to make room for:

```cpp
s[right]
```

---

## 🔥 Key Insight

A valid window contains:

```text
No Duplicate Characters
```

Whenever the window becomes invalid:

```text
Shrink from Left
Until Valid Again
```

Then continue expanding.

---

## 🎯 Interview One-Liner

Maintain a sliding window with unique characters using a hash set; expand with `right`, shrink with `left` whenever a duplicate appears.

---

## 💻 Code (C++)

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {

        unordered_set<char> st;

        int left = 0;
        int longest = 0;

        for(int right = 0; right < s.size(); right++) {

            while(st.find(s[right]) != st.end()) {

                st.erase(s[left]);
                left++;
            }

            st.insert(s[right]);

            int len = right - left + 1;

            longest = max(longest, len);
        }

        return longest;
    }
};
```
