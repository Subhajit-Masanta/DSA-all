# 00_Recursion_Introduction.md

# Recursion Basics

| Topic                  | Difficulty | Pattern      |
| :--------------------- | :--------- | :----------- |
| Recursion Introduction | Easy       | Fundamentals |

---

# 📌 Problem Description

Understand the basic idea of Recursion, the Call Stack, Base Case, Recursive Case, and the difference between the Call Phase and Return Phase.

---

# 🧠 Core Insight

## What is Recursion?

> **Recursion is a programming technique where a function solves a problem by calling itself on a smaller version of the same problem until a stopping condition (Base Case) is reached.**

Instead of thinking

```
Function calls itself.
```

think

```
Current function

↓

Solves one small task

↓

Delegates the remaining smaller problem

↓

Gets control back
```

Every recursive function follows this idea.

---

## Components of Recursion

Every recursive function has two mandatory parts.

```
Function()

{

    Base Case

    Recursive Case

}
```

### Base Case

The stopping condition.

```
if(problem is smallest)
    return;
```

Without a base case,

```
function()

↓

function()

↓

function()

↓

Infinite Recursion

↓

Stack Overflow
```

---

### Recursive Case

The function calls itself with a smaller problem.

```
function(n)

↓

function(n-1)

↓

function(n-2)

↓

Base Case
```

Each call must move closer to the base case.

---

# 📚 Call Stack

Every function call gets its own

* Parameters
* Local Variables
* Memory

These are stored inside the **Call Stack (LIFO - Last In First Out).**

Example

```
print(3)

↓

print(2)

↓

print(1)

↓

print(0)
```

Call Stack

```
TOP

print(0)

print(1)

print(2)

print(3)

BOTTOM
```

When the base case finishes,

```
print(0)

↓

print(1)

↓

print(2)

↓

print(3)
```

Functions return in reverse order.

---

# 🔥 Golden Rule

Every recursive function has **two phases**.

```
Call Phase

↓

Recursive Call

↓

Return Phase
```

---

## Code BEFORE Recursion

```
cout << n;

recursiveCall();
```

Execution

```
Call Phase

↓

Current statement executes

↓

Go deeper

↓

Base Case
```

Example Output

```
3
2
1
```

---

## Code AFTER Recursion

```
recursiveCall();

cout << n;
```

Execution

```
Go deeper

↓

Base Case

↓

Return Phase

↓

Current statement executes
```

Example Output

```
1
2
3
```

---

# ⏱ Time Complexity

Depends on the problem.

For single recursion visiting every element once,

```
O(n)
```

---

# 📦 Space Complexity

Maximum recursive depth

```
O(n)
```

because every function call occupies stack memory.

---

# ⚠️ Common Mistakes

❌ No Base Case

❌ Problem size not decreasing

❌ Confusing Call Phase with Return Phase

❌ Thinking recursion executes only top to bottom

---

# 🔥 Key Insight

```
Code BEFORE recursion

↓

Executes while going DOWN

(Call Phase)

--------------------------------

Code AFTER recursion

↓

Executes while coming BACK

(Return Phase)
```

---

# 🎯 Interview One-Liner

> "Recursion solves a problem by reducing it into smaller subproblems, where statements before the recursive call execute during the Call Phase and statements after the recursive call execute during the Return Phase."

---

# 📚 Pattern Recognition

Ask these four questions:

```
1. What is the function responsible for?

2. What is the smallest problem?

3. How is the problem becoming smaller?

4. Should my work happen
   BEFORE recursion
   or
   AFTER recursion?
```
