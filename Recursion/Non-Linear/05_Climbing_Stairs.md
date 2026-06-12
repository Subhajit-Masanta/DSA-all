
| Topic           | Difficulty | Pattern                    |
| :-------------- | :--------- | :------------------------- |
| Climbing Stairs | Easy       | Tree Recursion / Fibonacci |

---

# 📌 Problem Description

You are climbing a staircase of `n` steps.

At every move, you have only **2 choices**:

* Climb **1 step**
* Climb **2 steps**

Return the total number of distinct ways to reach the top.

---

# 🧠 Core Insight

## Don't Think Forward

Instead of asking

```
How can I climb from 0 to n?
```

think

```
How can I reach stair n?
```

There are only two possibilities.

```
Last move = 1 step

OR

Last move = 2 steps
```

---

## Recurrence Relation

To reach stair `n`

```
Come from stair (n-1)

OR

Come from stair (n-2)
```

Therefore

```
climb(n)

=

climb(n-1)

+

climb(n-2)
```

This is exactly the Fibonacci pattern.

---

# ⚙️ Algorithm

```
if(n==1)

return 1

---------------------

if(n==2)

return 2

---------------------

return

climb(n-1)

+

climb(n-2)
```

---

# 🌳 Recursion Tree

Example

```
n = 4
```

```
                  climb(4)
                 /        \
           climb(3)     climb(2)
           /      \
     climb(2)   climb(1)
```

Notice

```
climb(2)
```

is calculated twice.

This is called an **overlapping subproblem**.

---

# 🧪 Dry Run

## Input

```
n = 4
```

---

### Call Phase

```
climb(4)

↓

climb(3)

↓

climb(2)

↓

Base Case

return 2
```

```
climb(1)

↓

Base Case

return 1
```

---

### Return Phase

```
climb(3)

=

climb(2)

+

climb(1)

=

2

+

1

=

3
```

```
climb(2)

=

2
```

Finally

```
climb(4)

=

climb(3)

+

climb(2)

=

3

+

2

=

5
```

Answer

```
5
```

---

# 🧪 Actual Ways for n = 4

```
1 + 1 + 1 + 1

1 + 1 + 2

1 + 2 + 1

2 + 1 + 1

2 + 2
```

Total

```
5 ways
```

---

# 🧠 Why Base Cases Don't Make More Calls

```
climb(4)

↓

climb(3)

↓

climb(2)

↓

Base Case

↓

return 2

↓

STOP
```

`climb(2)` does **not** call

```
climb(1)

+

climb(0)
```

because it immediately returns.

---

# 🌳 Leaf Nodes

```
                climb(4)
               /        \
         climb(3)      climb(2)
         /      \          |
   climb(2)   climb(1)     |
      |          |          |
   return2   return1    return2
```

Base cases are **leaf nodes**.

They have **no children**.

---

# 💻 Code (C++)

```cpp
class Solution {
public:

    int climbStairs(int n)
    {
        if(n==1)
            return 1;

        if(n==2)
            return 2;

        return climbStairs(n-1)+climbStairs(n-2);
    }
};
```

---

# ⏱ Time Complexity

Every function creates **two recursive calls**.

```
T(n)

=

T(n-1)

+

T(n-2)
```

Therefore

```
O(2ⁿ)
```

Naive recursion repeatedly solves the same subproblems.

---

# 📦 Space Complexity

Maximum recursion depth

```
n
```

Therefore

```
O(n)
```

---

# ⚠️ Common Mistakes

❌ Thinking

```
climb(2)

↓

climb(1)+climb(0)
```

No.

`climb(2)` is a base case and immediately returns.

---

❌ Thinking recursion counts paths one by one.

It doesn't.

It counts

```
Ways from n-1

+

Ways from n-2
```

---

❌ Forgetting why addition is used.

To reach stair `n`, the last move is either

```
1 step

OR

2 steps
```

So both possibilities are added.

---

# 🔥 Key Insight

```
Current Stair

↓

Can be reached from

↓

(n-1)

OR

(n-2)

↓

Answer

=

f(n-1)

+

f(n-2)
```

---

# 🎯 Interview One-Liner

> **Climbing Stairs is Fibonacci in disguise: the number of ways to reach stair `n` equals the sum of the ways to reach stairs `n-1` and `n-2`.**

---

# 📚 Pattern Recognition

```
Current State

↓

Can come from

Previous State

OR

Previous Previous State

↓

f(n)

=

f(n-1)

+

f(n-2)
```

### Similar Problems

```
✅ Fibonacci

✅ Climbing Stairs

✅ Count Ways

✅ Decode Ways

✅ House Robber (DP)
```
