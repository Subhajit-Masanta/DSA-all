# 8. String to Integer (atoi)

| Topic   | Difficulty | Pattern                     |
| :------ | :--------- | :-------------------------- |
| Strings | Medium     | String Simulation / Parsing |

---

# 📌 Problem Description

Convert a string into a 32-bit signed integer.

Rules:

1. Ignore leading spaces.
2. Check sign (`+` or `-`).
3. Read digits only.
4. Stop at first non-digit character.
5. Handle overflow:

   * Greater than INT_MAX → return INT_MAX
   * Less than INT_MIN → return INT_MIN

---

# 🧠 Core Insight

We scan the string from left to right using one pointer.

We do NOT create a new string.

We do NOT use sliding window.

We simply:

```text
Skip Spaces
↓
Check Sign
↓
Read Digits
↓
Build Number
↓
Handle Overflow
```

---

# ⚙️ Algorithm

### Step 1

Skip leading spaces.

Example:

```text
"    42"
```

Move pointer until:

```text
42
^
```

---

### Step 2

Check sign.

```text
+ → positive
- → negative
```

Default:

```text
positive
```

---

### Step 3

Read digits.

While:

```cpp
isdigit(s[i])
```

Convert character into integer:

```cpp
int digit = s[i] - '0';
```

---

### Step 4

Build number.

Formula:

```cpp
num = num * 10 + digit;
```

Example:

```text
num = 42
digit = 7

42 * 10 + 7

427
```

---

### Step 5

Check overflow.

If:

```cpp
sign * num > INT_MAX
```

Return:

```cpp
INT_MAX
```

If:

```cpp
sign * num < INT_MIN
```

Return:

```cpp
INT_MIN
```

---

### Step 6

Return final answer.

```cpp
return sign * num;
```

---

# 🧪 Dry Run

## Example 1

Input:

```text
"   -042abc"
```

### Skip Spaces

```text
-042abc
^
```

### Sign

```text
negative
```

### Read Digits

```text
0
4
2
```

Stop at:

```text
a
```

### Number

```text
42
```

Apply sign:

```text
-42
```

Answer:

```text
-42
```

---

## Example 2

Input:

```text
"1337c0d3"
```

Read:

```text
1337
```

Stop at:

```text
c
```

Answer:

```text
1337
```

---

## Example 3

Input:

```text
"words and 987"
```

First character:

```text
w
```

Not:

```text
space
sign
digit
```

Answer:

```text
0
```

---

# ⏱ Time Complexity

```text
O(n)
```

Single traversal.

---

# 📦 Space Complexity

```text
O(1)
```

Only variables used.

---

# ⚠️ Common Mistakes

### Mistake 1

Forgetting to skip spaces.

```text
"   42"
```

Should return:

```text
42
```

---

### Mistake 2

Wrong sign handling.

```text
"-42"
```

Should return:

```text
-42
```

---

### Mistake 3

Not stopping at first non-digit.

```text
"1337abc"
```

Should return:

```text
1337
```

---

### Mistake 4

Ignoring overflow.

```text
"999999999999999"
```

Should return:

```text
2147483647
```

---

# 🔥 Key Formula

Convert character to integer:

```cpp
char ch = '7';

int digit = ch - '0';
```

Result:

```text
7
```

---

Build number:

```cpp
num = num * 10 + digit;
```

Example:

```text
42 + 7

427
```

---

# 🎯 Interview One-Liner

Use one pointer to skip spaces, process sign, read digits, build the number, and clamp the result within the 32-bit integer range.

---

# 💻 Code (C++)

```cpp
class Solution {
public:
    int myAtoi(string s) {

        int i = 0;
        int sign = 1;
        long long num = 0;

        while(i < s.size() && s[i] == ' ') {
            i++;
        }

        if(i < s.size() && (s[i] == '+' || s[i] == '-')) {
            if(s[i] == '-')
                sign = -1;
            i++;
        }

        while(i < s.size() && isdigit(s[i])) {

            int digit = s[i] - '0';

            num = num * 10 + digit;

            if(sign * num >= INT_MAX)
                return INT_MAX;

            if(sign * num <= INT_MIN)
                return INT_MIN;

            i++;
        }

        return sign * num;
    }
};
```

---

# 📝 Notebook One-Liner

```text
Skip spaces → Read sign → Read digits → Build number using num = num*10 + digit → Handle overflow → Return sign*num.
```
