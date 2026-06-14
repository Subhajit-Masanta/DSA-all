| Topic                      | Difficulty | Pattern                                  |
| :------------------------- | :--------- | :--------------------------------------- |
| Partition Equal Subset Sum | Medium     | Pick / Not Pick (Boolean Recursion → DP) |

---

# 📌 Problem Description

Given an array, determine whether it can be divided into **two subsets having equal sum**.

Example

```text
Input

[1,5,11,5]

Output

true

Explanation

[1,5,5]

[11]
```

---

# 🧠 Core Insight

At first, we may think

```text
Current Element

↓

Put in Left

OR

Put in Right
```

Maintain

```text
leftSum

rightSum
```

and finally check

```text
leftSum == rightSum
```

✅ Correct idea.

---

# ⭐ Better Observation

```text
leftSum + rightSum = TotalSum
```

If

```text
leftSum == rightSum
```

then

```text
leftSum

=

TotalSum / 2
```

So we only need to find **ONE subset** having sum

```text
target = TotalSum / 2
```

If total sum is odd,

```text
return false
```

immediately.

---

# 🌳 Pattern

Exactly the same as Subsets.

```text
Current Element

↓

Take

OR

Leave
```

Difference

```text
Subsets

↓

Store Answer

--------------------

Partition

↓

Return true / false
```

---

# 🌳 Decision Tree

Example

```text
arr = [1,5,11,5]

target = 11
```

```text
                     sum=0
                   /       \
              Take1      Skip1
               sum=1      sum=0
              /    \
         Take5    Skip5
          sum=6    sum=1
```

Every node stores

```text
(index,currentSum)
```

---

# ⚙️ Algorithm

```text
Current Element

↓

Take

↓

currentSum + arr[index]

OR

Leave

↓

currentSum

↓

If any branch reaches target

↓

return true
```

---

# 🧪 Dry Run

```text
arr = [1,5,11,5]

target = 11
```

Start

```text
solve(0,0)
```

Path

```text
Take 1

↓

sum = 1

↓

Take 5

↓

sum = 6

↓

Skip 11

↓

sum = 6

↓

Take 5

↓

sum = 11

↓

return true
```

---

# 🔄 Return Flow

```text
solve(3,11)

↓

true

↑

solve(2,6)

↓

take || skip

↓

true

↑

solve(1,1)

↓

true

↑

solve(0,0)

↓

true
```

Answer bubbles back to the first call.

---

# 💻 Code (C++)

```cpp
class Solution {
public:

    bool solve(int index,
               int curr,
               int total,
               vector<int>& arr)
    {
        if(curr == total/2)
            return true;

        if(index == arr.size())
            return false;

        if(curr > total/2)
            return false;

        bool take =
            solve(index+1,
                  curr+arr[index],
                  total,
                  arr);

        bool skip =
            solve(index+1,
                  curr,
                  total,
                  arr);

        return take || skip;
    }

    bool equalPartition(vector<int>& arr)
    {
        int total = 0;

        for(int x : arr)
            total += x;

        if(total % 2 == 1)
            return false;

        return solve(0,0,total,arr);
    }
};
```

---

# ⏱ Time Complexity

Each element has

```text
Take

OR

Leave
```

Therefore

```text
O(2ⁿ)
```

This brute-force recursion tries every possible subset.

---

# 📦 Space Complexity

```text
Recursion Depth

↓

n

↓

O(n)
```

---

# ⚠️ Why TLE?

The same state is solved repeatedly.

Example

```text
solve(2,6)

↓

computed

↓

computed again

↓

computed again
```

Tree

```text
               solve(0,0)

             /           \

      solve(1,1)     solve(1,0)

          \              /

           \            /

             solve(2,6)
```

This is called

```text
Overlapping Subproblems
```

and is the reason for Time Limit Exceeded.

---

# 🔥 Key Insight

```text
Subsets

↓

Take / Leave

↓

Store Vector

------------------------

Partition

↓

Take / Leave

↓

Return Boolean

------------------------

Same Pick / Not Pick Pattern
```

---

# 🎯 Interview One-Liner

> **Partition Equal Subset Sum is simply a Subset Sum problem: if the total sum is even, recursively check whether any subset can achieve `total/2`.**

---

# 📚 Pattern Recognition

```text
Subsets

↓

Subset Sum

↓

Partition Equal Subset Sum

↓

Memoization (DP)

↓

0/1 Knapsack
```

This problem is the **bridge between Recursion and Dynamic Programming**.
