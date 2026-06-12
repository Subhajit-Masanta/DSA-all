# 02_Palindrome_Number.md

| Topic             | Difficulty | Pattern                    |
| :---------------- | :--------- | :------------------------- |
| Palindrome Number | Easy       | Recursion / Two Approaches |

---

# 📌 Problem Description

Given an integer `n`, determine whether it is a palindrome.

Examples

```
12121  -> true

12321  -> true

12345  -> false

-121   -> true (ignore sign)
```

---

# 🧠 Core Insight

There are two recursive approaches.

## Method 1 (Recommended)

```
Number

↓

Convert to String

↓

Compare First & Last

↓

Solve Smaller Problem
```

Simple and easy to understand.

---

## Method 2 (Pure Number Recursion)

```
Take Last Digit

↓

Build Reverse Number

↓

Compare with Original
```

This method teaches how recursion can manipulate numbers.

---

# -----------------------------------------------------------------------------

# Method 1 : Convert Number to String

## Idea

```
12321

↓

"12321"

↓

Compare

1 == 1

↓

Compare

2 == 2

↓

Compare

3

↓

Base Case
```

---

# ⚙️ Algorithm

```
Convert number into string

↓

Compare left and right characters

↓

If equal

↓

Move inward

(left+1 , right-1)

↓

Repeat

↓

Base Case

left >= right
```

---

# 🌳 Recursion Tree

```
check("12321")

|

+-- check("232")

      |

      +-- check("3")

             |

             Base Case
```

---

# 🧪 Dry Run

Input

```
12321
```

Call

```
solve(0,4)

↓

1 == 1

↓

solve(1,3)

↓

2 == 2

↓

solve(2,2)

↓

left >= right

↓

true
```

Return

```
true

↑

true

↑

true
```

Answer

```
Palindrome
```

---

# 💻 Code (C++)

```cpp
class Solution {
public:

    bool solve(string &s, int left, int right)
    {
        if(left >= right)
            return true;

        if(s[left] != s[right])
            return false;

        return solve(s, left + 1, right - 1);
    }

    bool isPalindrome(int n)
    {
        n = abs(n);

        string s = to_string(n);

        return solve(s, 0, s.size() - 1);
    }
};
```

---

# ⏱ Time Complexity

```
O(n)

n = number of digits
```

---

# 📦 Space Complexity

```
O(n)

Recursive Call Stack
```

---

# -----------------------------------------------------------------------------

# Method 2 : Pure Number Recursion

## Idea

Build the reverse number recursively.

```
1234

↓

Take 4

↓

Take 3

↓

Take 2

↓

Take 1

↓

Reverse becomes

4321
```

---

# ⚙️ Algorithm

```
rev = rev * 10 + lastDigit

↓

Remove last digit

↓

Solve smaller problem

↓

Base Case

n == 0
```

---

# 🌳 Recursion Tree

```
reverse(1234)

|

+-- reverse(123)

      |

      +-- reverse(12)

             |

             +-- reverse(1)

                    |

                    +-- reverse(0)

                           |

                        Base Case
```

---

# 🧪 Dry Run

Initial

```
n = 1234

rev = 0
```

---

Call 1

```
digit = 4

rev = 4
```

---

Call 2

```
digit = 3

rev = 43
```

---

Call 3

```
digit = 2

rev = 432
```

---

Call 4

```
digit = 1

rev = 4321
```

---

Call 5

```
n = 0

Base Case

return
```

---

Final

```
original = 1234

reverse = 4321

Not Palindrome
```

---

Example

```
n = 12321

rev

0

↓

1

↓

12

↓

123

↓

1232

↓

12321

Palindrome
```

---

# 💻 Code (C++)

```cpp
class Solution {
public:

    void reverseNumber(int n, int &rev)
    {
        if(n == 0)
            return;

        rev = rev * 10 + (n % 10);

        reverseNumber(n / 10, rev);
    }

    bool isPalindrome(int n)
    {
        n = abs(n);

        int original = n;
        int rev = 0;

        reverseNumber(n, rev);

        return original == rev;
    }
};
```

---

# ⏱ Time Complexity

```
O(n)

n = number of digits
```

---

# 📦 Space Complexity

```
O(n)

Recursive Call Stack
```

---

# ⚠️ Common Mistakes

❌ Forgetting to ignore negative sign

```cpp
n = abs(n);
```

---

❌ Using

```
left > right
```

instead of

```
left >= right
```

for string recursion.

---

❌ Forgetting to pass

```
rev

by reference (&)
```

in numeric recursion.

---

# 🔥 Key Insight

```
Method 1

Number

↓

String

↓

Compare Ends

↓

Smaller String

----------------------------

Method 2

Number

↓

Take Last Digit

↓

Build Reverse

↓

Compare with Original
```

---

# 🎯 Interview One-Liner

> **For beginners, the string recursion approach is easier to derive and explain. The numeric recursion approach is useful for understanding recursion with state and parameter updates.**

---

# 📚 Pattern Recognition

```
Palindrome String

↓

Compare Ends

↓

Recursive Smaller Problem

----------------------------

Palindrome Number

↓

Convert to String

OR

↓

Build Reverse Number
```
