# Subset Sum Problem

| Topic | Difficulty | Pattern |
| ----- | ---------- | ------- |
| Dynamic Programming | Medium | 0/1 Knapsack — Take / Don't Take |

---

# 📌 Problem Description

Given an array of positive integers `arr[]` and a target `sum`, determine whether there exists a **subset** of the array whose elements add up exactly to `sum`.

Each element can be used:

```text
0 times
OR
1 time
```

It cannot be reused.

Return:

```text
true
```

if such a subset exists, otherwise:

```text
false
```

---

# Example

### Example 1

Input:

```text
arr = [3, 34, 4, 12, 5, 2]
sum = 9
```

Output:

```text
true
```

Explanation:

```text
3 + 4 + 2 = 9
```

Therefore, a valid subset exists.

---

### Example 2

Input:

```text
arr = [3, 34, 4, 12, 5, 2]
sum = 30
```

Output:

```text
false
```

No subset can produce exactly `30`.

---

### Example 3

Input:

```text
arr = [1, 2, 3]
sum = 6
```

Output:

```text
true
```

Because:

```text
1 + 2 + 3 = 6
```

---

# 🧠 Intuition

This is essentially a **boolean version of 0/1 Knapsack**.

In 0/1 Knapsack:

```text
TAKE
OR
DON'T TAKE
```

and we maximize value.

Here:

```text
TAKE
OR
DON'T TAKE
```

but we only ask:

```text
Can I make the target sum?
```

Therefore the DP stores:

```text
true / false
```

---

# Key Observation

Define:

```text
dp[i][j]
```

as:

> Can we form sum `j` using the first `i` elements?

For example:

```text
dp[3][7]
```

means:

> Can we form sum `7` using the first 3 elements?

The answer is:

```text
true
OR
false
```

---

# Two Choices

Suppose the current element is:

```text
arr[i-1]
```

We have two choices.

---

## Choice 1 — Don't Take

Don't use the current element.

The target remains:

```text
j
```

So:

```text
dp[i-1][j]
```

---

## Choice 2 — Take

If:

```text
arr[i-1] <= j
```

we can take the current element.

The remaining target becomes:

```text
j - arr[i-1]
```

So we check:

```text
dp[i-1][j-arr[i-1]]
```

We move to `i-1` because this is a **0/1** problem and the current element cannot be reused.

---

# Transition

If:

```text
arr[i-1] <= j
```

then:

```text
dp[i][j] =
    dp[i-1][j]
    ||
    dp[i-1][j-arr[i-1]]
```

In words:

```text
Don't take
    OR
Take
```

If the element is too large:

```text
arr[i-1] > j
```

we cannot take it.

Therefore:

```text
dp[i][j] = dp[i-1][j]
```

---

# Base Cases

## Target = 0

We can always make sum `0` by choosing the empty subset:

```text
{}
```

Therefore:

```text
dp[i][0] = true
```

for every `i`.

---

## No Elements

If there are no elements:

```text
dp[0][j] = false
```

for every positive `j`.

We cannot make a positive sum without elements.

Because the DP table is initialized with `false`, this is automatically handled.

---

# Why Do We Trust the Previous Row?

This is the most important concept.

Suppose:

```text
dp[i][j]
```

asks:

> Can I make `j` using the first `i` elements?

If we **don't take** the current element:

```text
arr[i-1]
```

then we only have the previous `i-1` elements.

So:

```text
dp[i-1][j]
```

already answers:

> Can the previous elements make `j`?

---

If we **take** the current element:

```text
arr[i-1]
```

then we have:

```text
j - arr[i-1]
```

left to make.

Again, only the previous elements are available.

Therefore:

```text
dp[i-1][j-arr[i-1]]
```

answers exactly the smaller problem we need.

So we are not blindly trusting the previous row.

The previous row represents a **smaller problem that has already been solved**.

---

# Example Table

For:

```text
arr = [3, 34, 4, 12, 5, 2]
sum = 9
```

The DP table becomes:

```text
             Target
        0  1  2  3  4  5  6  7  8  9
      ----------------------------------
0     | T  F  F  F  F  F  F  F  F  F
3     | T  F  F  T  F  F  F  F  F  F
34    | T  F  F  T  F  F  F  F  F  F
4     | T  F  F  T  T  F  F  T  F  F
12    | T  F  F  T  T  F  F  T  T  F
5     | T  F  F  T  T  T  F  T  T  T
2     | T  F  T  T  T  T  T  T  T  T
```

Final cell:

```text
dp[6][9] = true
```

Therefore:

```text
Answer = true
```

---

# How `dp[6][9]` Becomes True

The last element is:

```text
2
```

We want:

```text
9
```

Take `2`.

Remaining:

```text
9 - 2 = 7
```

So we check:

```text
dp[5][7]
```

which is:

```text
true
```

That means the first five elements can make `7`.

One way is:

```text
3 + 4 = 7
```

Therefore:

```text
3 + 4 + 2 = 9
```

So:

```text
dp[6][9] = true
```

---

# Algorithm

```text
Create dp[n+1][sum+1]

↓

Set dp[i][0] = true

↓

For every item i:

    For every target j:

        If arr[i-1] <= j:

            Don't Take:
            dp[i-1][j]

            Take:
            dp[i-1][j-arr[i-1]]

            Combine using OR

        Else:

            Copy previous answer

↓

Return dp[n][sum]
```

---

# Code — Tabulation

```cpp
class Solution {
public:

    bool isSubsetSum(vector<int>& arr, int sum)
    {
        int n = arr.size();

        vector<vector<bool>>
            dp(n + 1,
               vector<bool>(sum + 1, false));

        // Sum 0 is always possible
        // using the empty subset.
        for(int i = 0; i <= n; i++)
        {
            dp[i][0] = true;
        }

        for(int i = 1; i <= n; i++)
        {
            for(int j = 1; j <= sum; j++)
            {
                int val = arr[i-1];

                if(val <= j)
                {
                    // Don't take OR Take
                    dp[i][j] =
                        dp[i-1][j] ||
                        dp[i-1][j-val];
                }
                else
                {
                    // Current element is too large
                    dp[i][j] =
                        dp[i-1][j];
                }
            }
        }

        return dp[n][sum];
    }
};
```

---

# Dry Run

Consider:

```text
arr = [3, 4, 2]
sum = 7
```

Initial:

```text
dp[0][0] = true
```

After considering `3`:

```text
Possible sums:

0
3
```

After considering `4`:

```text
Possible sums:

0
3
4
7
```

because:

```text
3 + 4 = 7
```

After considering `2`:

```text
Possible sums:

0
2
3
4
5
6
7
9
```

In particular:

```text
7 = true
```

Therefore:

```text
dp[3][7] = true
```

---

# Complexity

| Complexity | Value |
|------------|-------|
| Time | **O(n × sum)** |
| Space | **O(n × sum)** |

There are:

```text
(n + 1) × (sum + 1)
```

DP states.

Each state takes `O(1)` work.

---

# Space Optimization

Each row only depends on the previous row:

```text
dp[i-1]
```

Therefore, we can reduce the table to:

```text
dp[j]
```

However, because this is **0/1 Subset Sum**, iterate the target **backwards**.

```cpp
for(int j = sum; j >= arr[i]; j--)
```

This prevents using the same element multiple times.

---

# Code — Space Optimized

```cpp
class Solution {
public:

    bool isSubsetSum(vector<int>& arr, int sum)
    {
        int n = arr.size();

        vector<bool> dp(sum + 1, false);

        dp[0] = true;

        for(int i = 0; i < n; i++)
        {
            for(int j = sum; j >= arr[i]; j--)
            {
                dp[j] =
                    dp[j] ||
                    dp[j-arr[i]];
            }
        }

        return dp[sum];
    }
};
```

---

# Why Backward Iteration?

Suppose:

```text
arr[i] = 3
```

and:

```text
sum = 6
```

If we iterate forward:

```text
j = 3
```

we can make:

```text
dp[3] = true
```

Then later:

```text
j = 6
```

we might use the newly updated:

```text
dp[3]
```

to make:

```text
dp[6] = true
```

This effectively uses the same `3` twice:

```text
3 + 3
```

But Subset Sum is **0/1**.

Each element can only be used once.

Therefore:

```text
j = sum → arr[i]
```

must go **backwards**.

---

# 0/1 Knapsack vs Subset Sum

| 0/1 Knapsack | Subset Sum |
|--------------|------------|
| Take / Don't Take | Take / Don't Take |
| Capacity | Target Sum |
| Maximize value | Check possibility |
| `max()` | `||` |
| `dp[i][w]` | `dp[i][sum]` |
| Item used once | Element used once |

---

# Recognition Pattern

If the problem says:

```text
"Does there exist a subset..."
```

and:

```text
"whose sum is exactly X?"
```

think:

```text
0/1 Subset Sum
```

Then ask:

```text
Can I TAKE this element?

Can I DON'T TAKE this element?

If I TAKE it:
    What sum remains?

If either possibility works:
    answer = true
```

---

# Connection With 0/1 Knapsack

The structures are almost identical.

### 0/1 Knapsack

```text
TAKE
    ↓
value + previous state
    ↓
MAX

DON'T TAKE
    ↓
previous state
```

### Subset Sum

```text
TAKE
    ↓
remaining target
    ↓
OR

DON'T TAKE
    ↓
same target
    ↓
OR
```

So:

```text
0/1 Knapsack
       ↓
Take / Don't Take
       ↓
Subset Sum
       ↓
Boolean DP
```

---

# Key Takeaways

- Subset Sum is a **0/1 Knapsack variation**.
- Every element can be used at most once.
- State:

```text
dp[i][j] =
whether sum j can be formed using first i elements
```

- For every element:
  - Take it
  - Don't take it
- If taking it:

```text
remaining = j - arr[i-1]
```

- Since this is a boolean problem, combine choices using:

```cpp
||
```

- `dp[i][0] = true` because the empty subset has sum `0`.
- In the space-optimized version, iterate sum **backwards**.
- The core transition is:

```cpp
dp[i][j] =
    dp[i-1][j] ||
    dp[i-1][j-arr[i-1]];
```

when the element fits.

The core pattern:

```text
                 ELEMENT
                    |
             ┌──────┴──────┐
             ↓             ↓
           TAKE        DON'T TAKE
             |             |
       target - value     target
             |             |
             ↓             ↓
        previous        previous
         state            state
             \             /
              \           /
                  OR
                   ↓
                dp[i][j]
```
