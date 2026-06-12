
| Topic              | Difficulty | Pattern                                  |
| :----------------- | :--------- | :--------------------------------------- |
| Count Good Numbers | Medium     | Math + Binary Exponentiation + Recursion |

---

# 📌 Problem Description

A digit string is **good** if:

* Digits at **even indices (0,2,4...)** are **even**
* Digits at **odd indices (1,3,5...)** are **prime (2,3,5,7)**

Return the total number of good strings of length `n`.

Since the answer can be huge, return it modulo `10^9 + 7`.

---

# 🧠 Core Insight

The question is about **indices**, not values.

Example

```
String : 2582

Digit  : 2   5   8   2
Index  : 0   1   2   3
```

Rules

```
Even Index (0,2,4...)

↓

Allowed Digits

0 2 4 6 8

↓

5 choices

----------------------------

Odd Index (1,3,5...)

↓

Allowed Digits

2 3 5 7

↓

4 choices
```

---

# 🧪 Examples

## n = 1

```
Index

0
```

Choices

```
0
2
4
6
8

Total = 5
```

Answer

```
5
```

---

## n = 2

```
Index

0   1
```

```
Index 0

5 choices

×

Index 1

4 choices

=

20
```

---

## n = 3

```
Index

0   1   2
```

```
5 × 4 × 5

=

100
```

---

## n = 4

```
Index

0   1   2   3
```

```
5 × 4 × 5 × 4

=

400
```

Matches the sample.

---

# 🧠 Pattern

```
n = 1

5

----------------

n = 2

5 × 4

----------------

n = 3

5 × 4 × 5

----------------

n = 4

5 × 4 × 5 × 4

----------------

n = 5

5 × 4 × 5 × 4 × 5
```

---

# 🧠 Formula

Even positions

```
(n+1)/2
```

Odd positions

```
n/2
```

Therefore

```
Answer

=

5^(even positions)

×

4^(odd positions)
```

or

```
5^((n+1)/2)

×

4^(n/2)
```

---

# ⚙️ Algorithm

```
Find even positions

↓

Find odd positions

↓

Compute

5^even

↓

Compute

4^odd

↓

Multiply

↓

Take modulo
```

---

# 🌳 Binary Exponentiation Tree

Example

```
power(5,3)
```

```
power(5,3)

odd

|

+---- power(5,1)

         odd

         |

         +---- power(5,0)

                    |

                 return 1
```

---

# 🧪 Dry Run

## Input

```
n = 5
```

---

### Step 1

```
even

=

(5+1)/2

=

3

----------------

odd

=

5/2

=

2
```

Need

```
5³ × 4²
```

---

### power(5,3)

Call Phase

```
power(5,3)

↓

power(5,1)

↓

power(5,0)

↓

return 1
```

Return Phase

```
power(5,1)

1×1×5

=

5

↑

power(5,3)

5×5×5

=

125
```

---

### power(4,2)

Call Phase

```
power(4,2)

↓

power(4,1)

↓

power(4,0)

↓

return 1
```

Return Phase

```
power(4,1)

1×1×4

=

4

↑

power(4,2)

4×4

=

16
```

---

Final Answer

```
125 × 16

=

2000
```

---

# 💻 Code (C++)

```cpp
class Solution {
public:

    long long MOD = 1000000007;

    long long power(long long x,long long n)
    {
        if(n==0)
            return 1;

        if(n%2==0)
        {
            long long h = power(x,n/2);

            return (h*h)%MOD;
        }
        else
        {
            long long h = power(x,n/2);

            return (((h*h)%MOD)*x)%MOD;
        }
    }

    int countGoodNumbers(long long n)
    {
        long long even = (n+1)/2;

        long long odd = n/2;

        return (power(5,even)*power(4,odd))%MOD;
    }
};
```

---

# ⏱ Time Complexity

Binary exponentiation divides the exponent by 2 every time.

```
n

↓

n/2

↓

n/4

↓

n/8

↓

...

↓

1
```

Therefore

```
O(log n)
```

---

# 📦 Space Complexity

Recursive depth

```
O(log n)
```

---

# ⚠️ Common Mistakes

❌ Thinking the condition is about digit value instead of index.

---

❌ Using

```
5 × 4 × 5 × 4 ...

```

with loops.

Constraint

```
n <= 10^15
```

will cause TLE.

---

❌ Forgetting modulo after multiplication.

---

❌ Calling

```cpp
power(x,n/2)
```

twice.

Store it once.

```cpp
long long h = power(x,n/2);
```

---

# 🔥 Key Insight

```
Independent Choices

↓

Multiply Choices

↓

5^evenPositions

×

4^oddPositions

↓

Huge Exponents

↓

Binary Exponentiation

↓

O(log n)
```

---

# 🎯 Interview One-Liner

> **Count choices independently, convert the answer into powers, and compute large exponents using Binary Exponentiation in O(log n).**

---

# 📚 Pattern Recognition

```
Every Position

↓

Fixed Independent Choices

↓

Multiplication Principle

↓

a^x × b^y

↓

Large Exponent

↓

Fast Power / Binary Exponentiation
```
