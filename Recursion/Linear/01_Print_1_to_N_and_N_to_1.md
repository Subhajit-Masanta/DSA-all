# 01_Print_1_to_N_and_N_to_1.md

| Topic                       | Difficulty | Pattern         |
| :-------------------------- | :--------- | :-------------- |
| Print 1 to N / Print N to 1 | Easy       | Basic Recursion |

---

# 📌 Problem Description

Print numbers

```
1 → N
```

and

```
N → 1
```

using Recursion.

---

# 🧠 Core Insight

The only difference is **where the work is performed**.

```
Work BEFORE recursion

↓

N → 1

----------------------------

Work AFTER recursion

↓

1 → N
```

---

# ⚙️ Algorithm

## Print 1 → N

```
Base Case

↓

Go deeper

↓

Print while returning
```

---

### Recursion Flow

```
print(3)

↓

print(2)

↓

print(1)

↓

return

↓

print 2

↓

return

↓

print 3
```

Output

```
1 2 3
```

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    void printTillN(int n) {

        if (n == 1) {
            cout << 1 << " ";
            return;
        }

        // Recursive Call
        printTillN(n - 1);

        // Executes during Return Phase
        cout << n << " ";
    }
};
```

---

## Print N → 1

```
Base Case

↓

Print current

↓

Go deeper
```

---

### Recursion Flow

```
print(3)

print 3

↓

print(2)

print 2

↓

print(1)

print 1

↓

return
```

Output

```
3 2 1
```

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    void printTill1(int n) {

        if (n == 1) {
            cout << 1 << " ";
            return;
        }

        // Executes during Call Phase
        cout << n << " ";

        // Recursive Call
        printTill1(n - 1);
    }
};
```

---

# 🧪 Dry Run

```
print(3)

↓

print(2)

↓

print(1)

↓

Base Case

↓

Return
```

```
Before recursion

↓

3 2 1

--------------------

After recursion

↓

1 2 3
```

---

# ⏱ Time Complexity

```
O(n)
```

---

# 📦 Space Complexity

```
O(n)
```

(Call Stack)

---

# ⚠️ Common Mistakes

❌ Printing after recursion when the question asks N → 1

❌ Printing before recursion when the question asks 1 → N

❌ Forgetting the Base Case

---

# 🔥 Key Insight

```
Need Increasing Order?

↓

Work AFTER recursion

(Return Phase)

--------------------------------

Need Decreasing Order?

↓

Work BEFORE recursion

(Call Phase)
```

---

# 🎯 Interview One-Liner

> **Print before recursion for decreasing order, print after recursion for increasing order.**

---

# 📚 Pattern Recognition

```
Question asks

1 → N

↓

Return Phase

↓

Code AFTER recursion

----------------------------

Question asks

N → 1

↓

Call Phase

↓

Code BEFORE recursion
```
