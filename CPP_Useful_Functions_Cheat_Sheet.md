# ⚡ C++ Useful Functions — DSA Cheat Sheet

A quick reference for the most useful C++ functions in Arrays, Strings, and OAs.

---

# 1. Character Checking Functions

Header:

```cpp
#include <cctype>
```

| Function | Checks |
|---|---|
| `isdigit(ch)` | Digit `0-9` |
| `isalpha(ch)` | Letter `a-z` or `A-Z` |
| `isalnum(ch)` | Letter or digit |
| `islower(ch)` | Lowercase letter |
| `isupper(ch)` | Uppercase letter |
| `isspace(ch)` | Space, tab, newline |
| `ispunct(ch)` | Punctuation |

Example:

```cpp
if (isdigit(ch)) {
    // digit
}

if (isalpha(ch)) {
    // letter
}

if (isalnum(ch)) {
    // letter or digit
}
```

### Remember

```text
digit   → isdigit()
letter  → isalpha()
both    → isalnum()
```

---

# 2. Uppercase and Lowercase

```cpp
ch = tolower(ch);
ch = toupper(ch);
```

Example:

```cpp
char ch = 'A';

ch = tolower(ch);   // 'a'
```

```cpp
char ch = 'b';

ch = toupper(ch);   // 'B'
```

⚠️ The returned value must be stored:

```cpp
ch = tolower(ch);   // ✅
tolower(ch);        // ❌ result ignored
```

---

# 3. Character ↔ Integer

## Digit Character to Integer

```cpp
char ch = '7';

int x = ch - '0';   // 7
```

## Integer to Digit Character

```cpp
int x = 7;

char ch = x + '0';  // '7'
```

---

## Letter to Index

```cpp
char ch = 'c';

int index = ch - 'a';   // 2
```

## Index to Letter

```cpp
int index = 2;

char ch = index + 'a';  // 'c'
```

### Remember

```text
'7' - '0' → 7
 7  + '0' → '7'

'c' - 'a' → 2
 2  + 'a' → 'c'
```

---

# 4. String ↔ Number

## String to Number

```cpp
int x = stoi(s);
```

Other conversions:

```cpp
stoi(s);    // string → int
stol(s);    // string → long
stoll(s);   // string → long long
stof(s);    // string → float
stod(s);    // string → double
```

## Number to String

```cpp
string s = to_string(x);
```

### Most Important

```text
stoi()      → string to int
stoll()     → string to long long
to_string() → number to string
```

---

# 5. String Size

```cpp
s.size();
s.length();
```

Both return the string length.

```cpp
string s = "hello";

cout << s.size();   // 5
```

Prefer:

```cpp
s.size()
```

---

# 6. Substring — `substr()`

Syntax:

```cpp
s.substr(start, length);
```

Example:

```cpp
string s = "abcdef";

string x = s.substr(2, 3);
```

Result:

```text
"cde"
```

From an index to the end:

```cpp
s.substr(2);
```

Result:

```text
"cdef"
```

### Example

```cpp
num.substr(0, i + 1);
```

Means:

```text
Start at index 0
Take i + 1 characters
```

---

# 7. Find — `find()`

```cpp
int pos = s.find("abc");
```

If found:

```text
Returns starting index
```

If not found:

```text
string::npos
```

Common pattern:

```cpp
if (s.find("abc") != string::npos) {
    // found
}
```

Not found:

```cpp
if (s.find("abc") == string::npos) {
    // not found
}
```

---

# 8. Reverse

```cpp
reverse(s.begin(), s.end());
```

Example:

```cpp
string s = "abc";

reverse(s.begin(), s.end());
```

Result:

```text
"cba"
```

Works for vectors:

```cpp
reverse(nums.begin(), nums.end());
```

---

# 9. Sort

Ascending:

```cpp
sort(nums.begin(), nums.end());
```

For string:

```cpp
sort(s.begin(), s.end());
```

Descending:

```cpp
sort(nums.rbegin(), nums.rend());
```

Custom descending:

```cpp
sort(nums.begin(), nums.end(), greater<int>());
```

---

# 10. Add Elements

## String

Add character:

```cpp
s.push_back('a');
```

or:

```cpp
s += 'a';
```

Add string:

```cpp
s += "hello";
```

## Vector

```cpp
nums.push_back(5);
```

---

# 11. Remove Elements

## Remove Last Element

String:

```cpp
s.pop_back();
```

Vector:

```cpp
nums.pop_back();
```

---

## Erase From String

```cpp
s.erase(index, count);
```

Example:

```cpp
string s = "abcdef";

s.erase(2, 2);
```

Result:

```text
"abef"
```

Remove one character:

```cpp
s.erase(i, 1);
```

---

## Erase From Vector

```cpp
nums.erase(nums.begin() + i);
```

Remove a range:

```cpp
nums.erase(nums.begin() + left,
           nums.begin() + right);
```

---

# 12. Clear and Empty

Delete everything:

```cpp
s.clear();
nums.clear();
mp.clear();
```

Check empty:

```cpp
s.empty();
nums.empty();
mp.empty();
```

Example:

```cpp
if (s.empty()) {
    return "";
}
```

---

# 13. Count Occurrences

```cpp
count(nums.begin(), nums.end(), x);
```

Example:

```cpp
vector<int> nums = {1, 2, 2, 3};

int x = count(nums.begin(), nums.end(), 2);
```

Result:

```text
2
```

For string:

```cpp
int x = count(s.begin(), s.end(), 'a');
```

---

# 14. Minimum and Maximum Element

```cpp
int smallest = *min_element(nums.begin(), nums.end());

int largest = *max_element(nums.begin(), nums.end());
```

⚠️ `*` is required because these functions return iterators.

```text
min_element() → iterator
*min_element() → actual value
```

---

# 15. Sum All Elements — `accumulate()`

Header:

```cpp
#include <numeric>
```

For `int`:

```cpp
int sum = accumulate(nums.begin(), nums.end(), 0);
```

For `long long`:

```cpp
long long sum =
    accumulate(nums.begin(), nums.end(), 0LL);
```

⚠️ Use `0LL` when the sum can be large.

---

# 16. Swap

```cpp
swap(a, b);
```

Example:

```cpp
int a = 5;
int b = 10;

swap(a, b);
```

Result:

```text
a = 10
b = 5
```

Common in:

```text
Two Pointers
Dutch National Flag
Sorting
Array manipulation
```

---

# 17. `min()` and `max()`

Two values:

```cpp
max(a, b);
min(a, b);
```

Multiple values:

```cpp
max({a, b, c});
min({a, b, c});
```

Example:

```cpp
int largest = max({x, y, z});
```

---

# 18. Remove Duplicates

For a sorted vector:

```cpp
sort(nums.begin(), nums.end());

nums.erase(
    unique(nums.begin(), nums.end()),
    nums.end()
);
```

Example:

```text
Before:
[1, 1, 2, 2, 3]

After:
[1, 2, 3]
```

⚠️ `unique()` alone does not shrink the vector.

Use:

```text
sort → unique → erase
```

---

# 19. Binary Search Functions

⚠️ Array must be sorted.

## Check if Element Exists

```cpp
binary_search(nums.begin(), nums.end(), target);
```

Returns:

```text
true / false
```

---

## Lower Bound

```cpp
auto it = lower_bound(nums.begin(), nums.end(), x);
```

Returns first position:

```text
>= x
```

Index:

```cpp
int index =
    lower_bound(nums.begin(), nums.end(), x)
    - nums.begin();
```

---

## Upper Bound

```cpp
auto it = upper_bound(nums.begin(), nums.end(), x);
```

Returns first position:

```text
> x
```

### Remember

```text
lower_bound → first >= x

upper_bound → first > x
```

---

# 20. Next Permutation

```cpp
next_permutation(nums.begin(), nums.end());
```

Example:

```text
[1, 2, 3]
    ↓
[1, 3, 2]
```

Previous permutation:

```cpp
prev_permutation(nums.begin(), nums.end());
```

---

# 21. Vector Search — `find()`

```cpp
auto it = find(nums.begin(), nums.end(), target);
```

Check:

```cpp
if (it != nums.end()) {
    // found
}
```

Get index:

```cpp
int index = it - nums.begin();
```

⚠️ This is `O(n)`.

---

# 22. Useful HashMap Functions

```cpp
unordered_map<int,int> mp;
```

Insert / update:

```cpp
mp[key] = value;
```

Frequency:

```cpp
mp[key]++;
```

Check exists:

```cpp
if (mp.find(key) != mp.end()) {
    // exists
}
```

Delete:

```cpp
mp.erase(key);
```

Size:

```cpp
mp.size();
```

Clear:

```cpp
mp.clear();
```

---

# 23. Useful Set Functions

```cpp
set<int> st;
```

Insert:

```cpp
st.insert(x);
```

Check exists:

```cpp
if (st.find(x) != st.end()) {
    // exists
}
```

Delete:

```cpp
st.erase(x);
```

Size:

```cpp
st.size();
```

---

# 🔥 Most Important Functions to Memorize

## Character

```cpp
isdigit(ch)
isalpha(ch)
isalnum(ch)
islower(ch)
isupper(ch)

tolower(ch)
toupper(ch)
```

## Conversion

```cpp
ch - '0'
x + '0'

ch - 'a'
index + 'a'

stoi(s)
stoll(s)
to_string(x)
```

## String

```cpp
s.size()
s.empty()

s.substr(start, length)
s.find(x)

s.push_back(ch)
s.pop_back()

s.erase(index, count)
s.clear()
```

## Array / Vector

```cpp
sort(v.begin(), v.end())

reverse(v.begin(), v.end())

count(v.begin(), v.end(), x)

*min_element(v.begin(), v.end())

*max_element(v.begin(), v.end())

accumulate(v.begin(), v.end(), 0)

swap(a, b)
```

## Binary Search Helpers

```cpp
binary_search()

lower_bound()

upper_bound()
```

---

# 🧠 Quick Recognizer

| Need | Function |
|---|---|
| Is digit? | `isdigit()` |
| Is letter? | `isalpha()` |
| Letter or digit? | `isalnum()` |
| Make lowercase | `tolower()` |
| Make uppercase | `toupper()` |
| Char digit → int | `ch - '0'` |
| Letter → index | `ch - 'a'` |
| String → int | `stoi()` |
| Int → string | `to_string()` |
| Get substring | `substr()` |
| Find in string | `find()` |
| Reverse | `reverse()` |
| Sort | `sort()` |
| Count value | `count()` |
| Minimum value | `*min_element()` |
| Maximum value | `*max_element()` |
| Sum array | `accumulate()` |
| Remove duplicates | `sort + unique + erase` |
| First `>= x` | `lower_bound()` |
| First `> x` | `upper_bound()` |

---

# Final Rule

```text
CHARACTER CHECK
→ isdigit, isalpha, isalnum

CHARACTER CONVERSION
→ tolower, toupper

STRING CONVERSION
→ stoi, stoll, to_string

STRING WORK
→ substr, find, erase

ARRAY WORK
→ sort, reverse, count

MIN / MAX / SUM
→ min_element, max_element, accumulate

SORTED SEARCH
→ lower_bound, upper_bound, binary_search
```