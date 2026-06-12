# 03_Pow_x_n_Binary_Exponentiation.md

| Topic     | Difficulty | Pattern                      |
| :-------- | :--------- | :--------------------------- |
| Pow(x, n) | Medium     | Recursion + Divide & Conquer |

---

# 📌 Problem Description

Implement

```
pow(x, n)
```

which returns

```
xⁿ
```

Examples

```
x = 2
n = 5

Output

32
```

```
x = 2
n = -2

Output

0.25
```

---

# 🧠 Core Insight

### Naive Idea

```
x⁵

=

x × x⁴
```

Problem size decreases by **1**.

```
5

↓

4

↓

3

↓

2

↓

1

↓

0
```

Time Complexity

```
O(n)
```

---

### Better Observation

If n is even,

```
x¹⁶

=

(x⁸)²
```

If n is odd,

```
x¹⁵

=

x × (x⁷)²
```

Now the problem size decreases by **half**.

```
16

↓

8

↓

4

↓

2

↓

1

↓

0
```

Time Complexity

```
O(log n)
```

---

# ⚙️ Algorithm

```
if n == 0

return 1

-------------------------

if n is even

half = power(x,n/2)

return half * half

-------------------------

if n is odd

half = power(x,n/2)

return half * half * x
```

---

# 🌳 Recursion Tree

Example

```
x = 2

n = 5
```

```
solve(2,5)

odd

|

+---- solve(2,2)

         even

         |

         +---- solve(2,1)

                    odd

                    |

                    +---- solve(2,0)

                               |

                           return 1
```

---

# 🧪 Dry Run

Input

```
x = 2

n = 5
```

---

## Call Phase

```
solve(2,5)

↓

solve(2,2)

↓

solve(2,1)

↓

solve(2,0)

↓

return 1
```

---

## Return Phase

### solve(2,1)

```
h = 1

return

1 × 1 × 2

=

2
```

---

### solve(2,2)

```
h = 2

return

2 × 2

=

4
```

---

### solve(2,5)

```
h = 4

return

4 × 4 × 2

=

32
```

---

Final Answer

```
32
```

---

# 💻 Code (C++)

```cpp
class Solution {
public:

    double solve(double x, long long n)
    {
        if (n == 0)
            return 1;

        if (n % 2 == 0)
        {
            double h = solve(x, n / 2);
            return h * h;
        }
        else
        {
            double h = solve(x, n / 2);
            return h * h * x;
        }
    }

    double myPow(double x, int n)
    {
        long long N = n;

        if (N < 0)
        {
            x = 1 / x;
            N = -N;
        }

        return solve(x, N);
    }
};
```

---

# ⏱ Time Complexity

Each recursive call divides the exponent by **2**.

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

Maximum recursive depth

```
log n
```

Therefore

```
O(log n)
```

---

# ⚠️ Common Mistakes

### ❌ Using int instead of double

```cpp
int h = solve(...);
```

Loses decimal precision.

Always use

```cpp
double h = solve(...);
```

---

### ❌ Calling recursion twice

```cpp
return solve(x,n/2) * solve(x,n/2);
```

This recomputes the same subproblem.

Instead

```cpp
double h = solve(x,n/2);

return h*h;
```

---

### ❌ Forgetting negative powers

```
2⁻²

=

1 / 2²

=

0.25
```

---

### ❌ Using int for INT_MIN

```
INT_MIN

=

-2147483648

-n

↓

Overflow
```

Always convert

```cpp
long long N = n;
```

before making it positive.

---

# 🔥 Key Insight

```
Naive Recursion

xⁿ

=

x × xⁿ⁻¹

↓

Problem decreases by 1

↓

O(n)

---------------------------------

Binary Exponentiation

Even

xⁿ

=

(xⁿ⁄²)²

Odd

xⁿ

=

(xⁿ⁄²)² × x

↓

Problem decreases by half

↓

O(log n)
```

---

# 🎯 Interview One-Liner

> **Whenever a problem can be reduced by half instead of one step, think Divide & Conquer or Binary Exponentiation, leading to O(log n) recursion.**

---

# 📚 Pattern Recognition

```
Question

↓

Can I reduce the problem by half?

↓

YES

↓

Compute half only once

↓

Reuse it

↓

O(log n)
```

### Similar Problems

```
✅ Binary Search

✅ Fast Power

✅ Segment Tree

✅ Merge Sort

✅ Divide & Conquer
```
