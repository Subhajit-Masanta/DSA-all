# 1910. Remove All Occurrences of a Substring

| Topic   | Difficulty | Pattern            |
| :------ | :--------- | :----------------- |
| Strings | Medium     | Simulation + Stack |

---

# 📌 Problem Description

Given:

```cpp
string s
string part
```

Repeatedly remove the **leftmost occurrence** of `part` from `s`.

Continue until `part` no longer exists.

Return the final string.

---

# Approach 1 : Simulation (find + erase)

## 🧠 Core Insight

Repeatedly:

1. Find the first occurrence.
2. Erase it.
3. Search again.

Continue until the substring no longer exists.

---

## ⚙️ Algorithm

### Step 1

Search:

```cpp
int pos = s.find(part);
```

---

### Step 2

If:

```cpp
pos == string::npos
```

No occurrence exists.

Return answer.

---

### Step 3

Otherwise erase.

```cpp
s.erase(pos, part.size());
```

---

### Step 4

Repeat.

---

## 🧪 Dry Run

Input

```text
s = daabcbaabcbc
part = abc
```

Find:

```text
daabcbaabcbc
  ^^^
```

Erase

```text
dabaabcbc
```

Again

```text
dabaabcbc
    ^^^
```

Erase

```text
dababc
```

Again

```text
dababc
   ^^^
```

Erase

```text
dab
```

No more `"abc"`.

Return

```text
dab
```

---

## 💻 Code

```cpp
class Solution {
public:
    string removeOccurrences(string s, string part) {

        while(true) {

            int pos = s.find(part);

            if(pos == string::npos)
                break;

            s.erase(pos, part.size());
        }

        return s;
    }
};
```

---

## Complexity

### Time

```text
O(n²)
```

Repeated `find()` and `erase()` operations may rescan the string many times in the worst case.

### Space

```text
O(1)
```

---

# Approach 2 : Stack (Recommended)

## 🧠 Core Insight

Instead of repeatedly modifying the original string,

build the answer character by character.

Treat a string as a stack.

After every push,

check whether the **suffix** equals `part`.

If yes,

pop those characters.

---

## ⚙️ Algorithm

### Step 1

Create an empty string.

```cpp
string st;
```

Use it as a stack.

---

### Step 2

Push every character.

```cpp
st.push_back(ch);
```

---

### Step 3

Whenever:

```cpp
st.size() >= part.size()
```

compare the last characters with `part`.

---

### Step 4

If they match,

pop exactly:

```cpp
part.size()
```

characters.

---

### Step 5

Continue until all characters are processed.

Return stack.

---

## 🧪 Dry Run

Input

```text
s = daabc
part = abc
```

Push

```text
d
da
daa
daab
daabc
```

Suffix:

```text
abc
```

Matches.

Pop

```text
c
b
a
```

Remaining

```text
da
```

Continue.

---

## 💻 Code

```cpp
class Solution {
public:
    string removeOccurrences(string s, string part) {

        string st;

        for(char ch : s) {

            st.push_back(ch);

            if(st.size() >= part.size()) {

                bool match = true;

                for(int i = 0; i < part.size(); i++) {

                    if(st[st.size() - part.size() + i] != part[i]) {
                        match = false;
                        break;
                    }
                }

                if(match) {

                    for(int i = 0; i < part.size(); i++)
                        st.pop_back();
                }
            }
        }

        return st;
    }
};
```

---

## Complexity

### Time

```text
O(n × m)
```

where:

```text
n = s.length()
m = part.length()
```

Each character is pushed once, and whenever the stack is long enough, up to `m` characters are compared with the suffix.

### Space

```text
O(n)
```

for the stack/string.

---

# Which Approach To Use?

| Approach     | Time     | Space | Interview |
| ------------ | -------- | ----: | :-------: |
| find + erase | O(n²)    |  O(1) |   ✅ Easy  |
| Stack        | O(n × m) |  O(n) |  ⭐ Better |

---

# Common Mistakes

### Mistake 1

Using

```cpp
find()
```

only once.

Need:

```cpp
while(...)
```

because removing one occurrence may create another.

---

### Mistake 2

Checking suffix before:

```cpp
st.size() >= part.size()
```

---

### Mistake 3

Using `substr()` every iteration.

Comparing characters directly avoids creating temporary strings.

---

# Pattern Recognition

Keywords

```text
Repeated Removal
Erase Substring
String Modification
```

Think

```text
Simulation

or

Stack + String Builder
```

---

# Interview One-Liner

Repeatedly removing substrings can be solved by simulation (`find` + `erase`) or more efficiently by treating the answer string as a stack and removing matching suffixes.

---

# 📝 Notebook One-Liner

```text
Simulation: find → erase → repeat.

Stack: Push every character, whenever the suffix equals part, pop part.size() characters.
```
