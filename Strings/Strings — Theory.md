# Strings — Theory Required for EY

> **EY Placement / Interview Prep — C++**

---

# 1. What is a String?

A string is a sequence of characters.

```cpp
string s = "hello";
```

Conceptually:

```text
h  e  l  l  o
0  1  2  3  4
```

Access:

```cpp
s[i]
```

### Complexity

```text
Access character → O(1)
Traversal         → O(n)
```

---

# 2. Why Do We Need Strings?

String problems mainly test:

* Character manipulation
* Traversal
* Searching
* Comparison
* Two Pointers
* Hashing
* Sliding Window
* Parsing

For EY, reported technical/coding rounds have included string manipulation and palindrome problems.

---

# 3. C++ String Basics

```cpp
#include <string>

string s = "hello";
```

### Length

```cpp
s.length();
s.size();
```

### Access

```cpp
s[0];
s.at(0);
```

### Modify

```cpp
s[0] = 'H';
```

### Append

```cpp
s += " world";
```

### Empty

```cpp
s.empty();
```

---

# 4. Important String Functions

Know these:

```cpp
s.length()
s.size()
s.empty()
s.push_back()
s.pop_back()
s.substr()
s.find()
s.erase()
s.insert()
```

Example:

```cpp
string s = "abcdef";

s.substr(1, 3);
```

Result:

```text
"bcd"
```

---

# 5. Character Functions

Know:

```cpp
isdigit(c)
isalpha(c)
isalnum(c)
islower(c)
isupper(c)
tolower(c)
toupper(c)
```

Example:

```cpp
if (isdigit(c)) {
    // digit
}
```

Useful for:

* Character validation
* Case conversion
* Palindrome
* String cleaning

---

# 6. ASCII

Characters have numeric codes.

Important concept:

```text
'A' → 65
'a' → 97
```

For lowercase English letters:

```cpp
int index = c - 'a';
```

Maps:

```text
a → 0
b → 1
c → 2
...
z → 25
```

This is extremely useful for frequency counting.

---

# 7. String Traversal

### Index-based

```cpp
for (int i = 0; i < s.size(); i++) {
    cout << s[i];
}
```

### Range-based

```cpp
for (char c : s) {
    cout << c;
}
```

Know both.

---

# 8. C++ String vs Character Array

### Character Array

```cpp
char s[] = "hello";
```

### C++ String

```cpp
string s = "hello";
```

C-style strings are null-terminated:

```text
h e l l o \0
```

For EY:

> Prefer `string` unless the interviewer specifically asks about C-style strings.

---

# 9. Important C++ Point: Strings Are Mutable

In C++:

```cpp
string s = "hello";

s[0] = 'H';
```

Valid.

Result:

```text
Hello
```

Therefore:

> **C++ `std::string` is mutable.**

Do not give the Java answer that "String is immutable" when interviewing in C++.

---

# 10. String Comparison

```cpp
string a = "abc";
string b = "abc";

if (a == b) {
    // equal
}
```

Lexicographical comparison:

```cpp
if (a < b) {
    // a comes before b
}
```

Example:

```text
"apple" < "banana"
```

---

# 11. Reverse a String ⭐⭐⭐⭐⭐

Example:

```text
hello
 ↓
olleh
```

### Two Pointer Idea

```text
left →          ← right
h e l l o
```

Swap:

```cpp
swap(s[left], s[right]);
```

Move:

```cpp
left++;
right--;
```

Complexity:

```text
Time  → O(n)
Space → O(1)
```

C++ built-in:

```cpp
reverse(s.begin(), s.end());
```

Know how to implement it manually.

---

# 12. Palindrome ⭐⭐⭐⭐⭐

A palindrome reads the same forward and backward.

Examples:

```text
madam
racecar
level
```

### Two Pointer

```text
left →          ← right
m a d a m
```

Compare:

```cpp
s[left] == s[right]
```

Then:

```cpp
left++;
right--;
```

If every pair matches → palindrome.

Complexity:

```text
Time  → O(n)
Space → O(1)
```

Palindrome has been explicitly reported as an EY GDS interview question.

---

# 13. Frequency Counting ⭐⭐⭐⭐⭐

Example:

```text
banana
```

Frequency:

```text
b → 1
a → 3
n → 2
```

### Using HashMap

```cpp
unordered_map<char, int> freq;

for (char c : s) {
    freq[c]++;
}
```

### Using Frequency Array

If only lowercase `a-z`:

```cpp
int freq[26] = {};

for (char c : s) {
    freq[c - 'a']++;
}
```

### Decision

```text
Only lowercase a-z
        ↓
int freq[26]

General character mapping
        ↓
unordered_map
```

---

# 14. Anagram ⭐⭐⭐⭐⭐

Two strings are anagrams if they contain the same characters with the same frequencies.

Example:

```text
listen
silent
```

Both contain:

```text
e → 1
i → 1
l → 1
n → 1
s → 1
t → 1
```

### Approach 1 — Sorting

```text
sort(s1)
sort(s2)

compare
```

Complexity:

```text
O(n log n)
```

### Approach 2 — Frequency

```text
count characters
compare frequencies
```

Complexity:

```text
O(n)
```

For lowercase English letters:

```cpp
int freq[26] = {};
```

---

# 15. First Non-Repeating Character ⭐⭐⭐⭐

Example:

```text
aabbcdde
```

Answer:

```text
c
```

Pattern:

```text
1. Count frequency
2. Traverse again
3. Find first frequency == 1
```

Complexity:

```text
Time  → O(n)
Space → O(1) / O(k)
```

---

# 16. Duplicate Characters ⭐⭐⭐⭐

Question:

> Find characters that appear more than once.

Use:

```cpp
unordered_map<char, int> freq;
```

or:

```cpp
int freq[26] = {};
```

Mental pattern:

```text
"How many times?"
        ↓
Frequency Map
```

---

# 17. Remove Duplicate Characters ⭐⭐⭐⭐

Example:

```text
"programming"
```

Need unique characters.

Use:

```cpp
unordered_set<char> st;
```

Mental pattern:

```text
"Have I seen this character?"
            ↓
       unordered_set
```

---

# 18. Two Pointers on Strings ⭐⭐⭐⭐⭐

General pattern:

```cpp
int left = 0;
int right = s.size() - 1;

while (left < right) {

    // process

    left++;
    right--;
}
```

Used for:

```text
Palindrome
Reverse
Compare both ends
Ignoring characters
```

---

# 19. Sliding Window on Strings ⭐⭐⭐⭐

Used for **substring** problems.

Typical signals:

```text
"longest substring"
"shortest substring"
"without repeating"
"at most K characters"
"frequency inside a window"
```

Classic example:

```text
Longest Substring Without Repeating Characters
```

Pattern:

```text
String
  +
Sliding Window
  +
HashMap / HashSet
```

You already know Sliding Window from arrays, so reuse that knowledge.

---

# 20. Substring vs Subsequence ⭐⭐⭐⭐

### Substring

Characters must be **contiguous**.

```text
"abcde"

"bcd" → substring
```

### Subsequence

Characters don't need to be contiguous, but order remains the same.

```text
"abcde"

"ace" → subsequence
```

Remember:

```text
Substring  → contiguous
Subsequence → not necessarily contiguous
```

---

# 21. String Searching

C++:

```cpp
s.find("abc");
```

Returns the starting position.

If not found:

```cpp
string::npos
```

Example:

```cpp
if (s.find("abc") != string::npos) {
    // found
}
```

For EY:

> Know `find()`. Do not spend time on KMP/Rabin-Karp unless you have extra preparation time.

---

# 22. Split / Tokenize a String

Use `stringstream`.

Example:

```text
"hello world from EY"
```

```cpp
stringstream ss(s);
string word;

while (ss >> word) {
    cout << word << '\n';
}
```

Output:

```text
hello
world
from
EY
```

Useful for sentence/word problems.

---

# 23. Reverse Words ⭐⭐⭐

Example:

```text
"I love coding"
```

Output:

```text
"coding love I"
```

Main concepts:

```text
String traversal
+
Tokenization
+
String manipulation
```

Know the approach, but this is lower priority than palindrome, anagram and frequency questions.

---

# 24. Case Conversion

```cpp
tolower(c);
toupper(c);
```

Useful for:

```text
Case-insensitive comparison
Palindrome
Anagram
String normalization
```

Example:

```text
"Madam"
   ↓
"madam"
```

---

# 25. String Complexity

For a string of length `n`:

| Operation           | Complexity |
| ------------------- | ---------: |
| Access `s[i]`       |       O(1) |
| Traverse            |       O(n) |
| Reverse             |       O(n) |
| Search character    |       O(n) |
| Frequency count     |       O(n) |
| Palindrome          |       O(n) |
| Anagram — frequency |       O(n) |
| Anagram — sorting   | O(n log n) |

---

# 26. String + Hashing ⭐⭐⭐⭐⭐

Very important combination.

```text
String
   ↓
Characters
   ↓
HashMap / HashSet
```

### Frequency

```text
"How many times?"
        ↓
HashMap / frequency array
```

### Duplicate

```text
"Have I seen this?"
        ↓
HashSet
```

### Anagram

```text
Same character frequencies?
        ↓
Frequency Map
```

### First non-repeating

```text
Frequency + second traversal
```

---

# 27. String + Two Pointers ⭐⭐⭐⭐⭐

```text
Palindrome
Reverse
Compare ends
```

Think:

```text
left ─────→
←───── right
```

---

# 28. String + Sliding Window ⭐⭐⭐⭐

If you see:

```text
"longest substring"
"shortest substring"
"without repeating"
"at most K"
```

Think:

```text
Sliding Window
      +
HashMap / HashSet
```

---

# 29. What NOT to Study for EY

Low priority:

```text
❌ KMP
❌ Rabin-Karp
❌ Z Algorithm
❌ Manacher's Algorithm
❌ Suffix Array
❌ Suffix Tree
❌ Advanced String DP
```

EY reports show basic/intermediate DSA being relevant, with strings appearing alongside arrays, linked lists and graphs.

Don't waste your limited preparation time trying to become a string-algorithms researcher.

---

# 30. EY String Theory Checklist

## ⭐⭐⭐⭐⭐ MUST KNOW

```text
✓ String basics
✓ String traversal
✓ Important C++ string functions
✓ Character functions
✓ ASCII
✓ Reverse String
✓ Palindrome
✓ Frequency Counting
✓ Anagram
✓ First Non-Repeating Character
✓ Two Pointers
✓ String + HashMap / HashSet
```

## ⭐⭐⭐⭐ SHOULD KNOW

```text
✓ Substring vs Subsequence
✓ Sliding Window
✓ Remove Duplicate Characters
✓ String Comparison
✓ Reverse Words
✓ stringstream / tokenization
✓ find()
```

## ⭐⭐ LOW PRIORITY

```text
✗ KMP
✗ Rabin-Karp
✗ Z Algorithm
✗ Manacher
✗ Suffix structures
```

---

# 31. Final Mental Model

```text
                       STRING
                          │
          ┌───────────────┼───────────────┐
          ↓               ↓               ↓
      Characters      Two Pointers      Hashing
          │               │               │
          ↓               ↓               ↓
      Frequency        Palindrome       Anagram
      ASCII             Reverse         Frequency
      Case              Compare         Duplicate
                                          │
                                          ↓
                                   Sliding Window
                                          │
                                          ↓
                                  Longest Substring
```

## 5-Second Decision Rules

```text
Palindrome?
→ Two Pointers

Reverse?
→ Two Pointers / reverse()

Frequency?
→ int[26] / unordered_map

Unique / Seen Before?
→ unordered_set

Anagram?
→ Frequency / Sorting

Longest / Shortest SUBSTRING?
→ Sliding Window + HashMap/Set

Need words from sentence?
→ stringstream

Need substring search?
→ find()
```

---

# EY STRING PREP — BOTTOM LINE

You don't need 50 string algorithms.

Get these **rock solid**:

```text
1. Traversal
2. Reverse
3. Palindrome
4. Frequency
5. Anagram
6. First non-repeating character
7. Duplicate characters
8. Two pointers
9. Sliding window
10. String + HashMap/HashSet
11. Substring vs subsequence
12. Basic C++ string functions
```

EY's reported coding rounds have included strings alongside arrays and other core DSA, while interview reports specifically include string manipulation and palindrome questions.
