# 0/1 Knapsack

| Topic | Difficulty | Pattern |
| ----- | ---------- | ------- |
| Dynamic Programming | Medium | 2D DP — 0/1 Knapsack / Take or Don't Take |

---

# 📌 Problem Description

You are given:

- `N` items
- `val[i]` → value of item `i`
- `wt[i]` → weight of item `i`
- `W` → maximum capacity of the knapsack

You need to select items such that:

```text
Total Weight <= W
```

and:

```text
Total Value
```

is maximized.

Each item can be used **at most once**.

This is why it is called:

```text
0/1 Knapsack
```

```text
0 → Don't take the item

1 → Take the item
```

---

# Example

Input:

```text
W = 7

val = [1, 4, 5, 7]

wt = [1, 3, 4, 5]
```

Possible choice:

```text
Item 1:
weight = 3
value = 4

Item 2:
weight = 4
value = 5
```

Total:

```text
weight = 3 + 4 = 7
value  = 4 + 5 = 9
```

So the maximum value is:

```text
9
```

---

# 🧠 Intuition

For every item, there are only two choices:

```text
TAKE
OR
DON'T TAKE
```

If we take the current item:

```text
value increases by val[i]
capacity decreases by wt[i]
```

If we don't take it:

```text
capacity remains the same
```

Because each item can be used only once, after taking item `i` we move to:

```text
i - 1
```

and never consider item `i` again.

---

# Key Observation

We need two pieces of information:

```text
1. Which items are available?
2. How much capacity is available?
```

Therefore we need a 2D state.

Define:

```text
dp[i][w]
```

as:

> Maximum value that can be obtained using items `0...i` with capacity `w`.

---

# Choices

For item `i` and capacity `w`:

## Choice 1 — Don't Take

If we don't take item `i`:

```text
dp[i][w] = dp[i-1][w]
```

We simply use the best answer from the previous items.

---

## Choice 2 — Take

We can take item `i` only if:

```text
wt[i] <= w
```

Then:

```text
value gained = val[i]
```

and remaining capacity becomes:

```text
w - wt[i]
```

Since this is **0/1 Knapsack**, item `i` cannot be reused.

Therefore we move to:

```text
i - 1
```

The candidate becomes:

```text
val[i] + dp[i-1][w-wt[i]]
```

---

# Transition

If:

```text
wt[i] <= w
```

then:

```text
dp[i][w] =
max(
    dp[i-1][w],
    val[i] + dp[i-1][w-wt[i]]
)
```

Otherwise:

```text
dp[i][w] = dp[i-1][w]
```

---

# Why `i-1` After Taking?

This is the most important part of **0/1 Knapsack**.

Suppose we take item `i`.

We cannot take it again.

Therefore:

```text
TAKE item i
     ↓
Move to item i-1
```

This ensures every item is used at most once.

Compare this with Unbounded Knapsack:

```text
TAKE item i
     ↓
Can potentially use item i again
```

That difference changes the DP transition.

---

# Algorithm — Memoization

```text
Create dp table initialized with -1

↓

solve(i, W)

↓

If no items remain
OR
capacity becomes 0

return 0

↓

If dp[i][W] already calculated

return dp[i][W]

↓

If wt[i] <= W

    TAKE:
    val[i] + solve(i-1, W-wt[i])

    DON'T TAKE:
    solve(i-1, W)

    Take maximum

↓

Otherwise

    DON'T TAKE

↓

Store answer in dp[i][W]

↓

Return answer
```

---

# Code — Memoization

```cpp
class Solution {
public:

    int solve(
        int W,
        vector<int>& val,
        vector<int>& wt,
        int n,
        vector<vector<int>>& dp
    )
    {
        if(W == 0 || n < 0)
            return 0;

        if(dp[n][W] != -1)
            return dp[n][W];

        if(wt[n] <= W)
        {
            int take =
                val[n] +
                solve(
                    W - wt[n],
                    val,
                    wt,
                    n - 1,
                    dp
                );

            int nottake =
                solve(
                    W,
                    val,
                    wt,
                    n - 1,
                    dp
                );

            return dp[n][W] =
                max(take, nottake);
        }

        return dp[n][W] =
            solve(
                W,
                val,
                wt,
                n - 1,
                dp
            );
    }

    int knapsack(
        int W,
        vector<int>& val,
        vector<int>& wt
    )
    {
        int n = wt.size();

        vector<vector<int>>
            dp(n, vector<int>(W + 1, -1));

        return solve(
            W,
            val,
            wt,
            n - 1,
            dp
        );
    }
};
```

---

# Complexity — Memoization

| Complexity | Value |
|------------|-------|
| Time | **O(N × W)** |
| Space | **O(N × W)** |

There are at most:

```text
N × W
```

different states.

Each state is calculated only once.

---

# Tabulation

Now convert the same state into a bottom-up table.

We use:

```text
dp[i][w]
```

where:

```text
i = current item
w = current capacity
```

---

# Base Cases

If capacity is `0`:

```text
dp[i][0] = 0
```

because no item can be taken.

If there are no items:

```text
dp[0][w]
```

depends on how we define the rows.

For easier tabulation, use:

```text
dp[i][w]
```

where `i` means:

> Maximum value using the **first `i` items**.

Then:

```text
dp[0][w] = 0
```

because there are zero items.

This makes the table cleaner than the recursive indexing.

---

# Tabulation State

```text
dp[i][w]

=

maximum value using the first i items
with capacity w
```

Therefore item `i-1` is the current item.

Its weight:

```text
wt[i-1]
```

Its value:

```text
val[i-1]
```

---

# Tabulation Transition

If:

```text
wt[i-1] <= w
```

we have two choices.

### Don't Take

```text
dp[i-1][w]
```

### Take

```text
val[i-1] + dp[i-1][w-wt[i-1]]
```

Therefore:

```text
dp[i][w] =
max(
    dp[i-1][w],
    val[i-1] + dp[i-1][w-wt[i-1]]
)
```

Otherwise:

```text
dp[i][w] = dp[i-1][w]
```

---

# Algorithm — Tabulation

```text
Create dp[n+1][W+1]

↓

Initialize first row with 0

↓

For i = 1 to n

    For w = 0 to W

        Don't take item:

        dp[i][w] = dp[i-1][w]

        If item fits:

            Take item:

            val[i-1]
            +
            dp[i-1][w-wt[i-1]]

            Take maximum

↓

Return dp[n][W]
```

---

# Code — Tabulation

```cpp
class Solution {
public:

    int knapsack(
        int W,
        vector<int>& val,
        vector<int>& wt
    )
    {
        int n = wt.size();

        vector<vector<int>>
            dp(n + 1, vector<int>(W + 1, 0));

        for(int i = 1; i <= n; i++)
        {
            for(int w = 0; w <= W; w++)
            {
                // Don't take the item
                dp[i][w] = dp[i-1][w];

                // Take the item if it fits
                if(wt[i-1] <= w)
                {
                    int take =
                        val[i-1] +
                        dp[i-1][w-wt[i-1]];

                    dp[i][w] =
                        max(dp[i][w], take);
                }
            }
        }

        return dp[n][W];
    }
};
```

---

# Dry Run

Consider:

```text
val = [4,5,7]

wt = [2,3,4]

W = 5
```

Table:

```text
        Capacity
        0  1  2  3  4  5
      -------------------
0     | 0  0  0  0  0  0
item1 | 0  0  4  4  4  4
item2 | 0  0  4  5  5  9
item3 | 0  0  4  5  7  9
```

Final:

```text
dp[3][5] = 9
```

The optimal choice is:

```text
item 1 + item 2
```

Weight:

```text
2 + 3 = 5
```

Value:

```text
4 + 5 = 9
```

---

# Space Optimization

Notice that each row only depends on the previous row:

```text
dp[i-1]
```

Therefore, the 2D table can be reduced to a 1D array.

But there is a **very important rule**.

For 0/1 Knapsack, capacity must be iterated:

```text
from W down to wt[i]
```

Why?

Because each item can only be used once.

If we iterate forward:

```text
w = wt[i] → W
```

we may accidentally use the same item multiple times.

---

# Code — Space Optimized

```cpp
class Solution {
public:

    int knapsack(
        int W,
        vector<int>& val,
        vector<int>& wt
    )
    {
        int n = wt.size();

        vector<int> dp(W + 1, 0);

        for(int i = 0; i < n; i++)
        {
            for(int w = W; w >= wt[i]; w--)
            {
                dp[w] =
                    max(
                        dp[w],
                        val[i] + dp[w-wt[i]]
                    );
            }
        }

        return dp[W];
    }
};
```

---

# Why Iterate Capacity Backwards?

Suppose:

```text
weight = 2
value = 5
```

and:

```text
W = 4
```

If we iterate forward:

```text
w = 2
```

we calculate:

```text
dp[2] = max(dp[2], 5 + dp[0])
      = 5
```

Then:

```text
w = 4
```

we could use the newly updated:

```text
dp[2]
```

again:

```text
dp[4] = 5 + dp[2]
      = 10
```

That effectively uses the same item twice.

But 0/1 Knapsack says:

```text
Each item → at most once
```

Therefore we iterate backwards:

```text
W → wt[i]
```

so the current item cannot reuse its newly updated state.

---

# 0/1 vs Unbounded Knapsack

This is extremely important.

| 0/1 Knapsack | Unbounded Knapsack |
|--------------|--------------------|
| Item used once | Item can be reused |
| Take or Don't Take | Take or Don't Take |
| 2D transition uses previous item | Can remain on same item |
| 1D capacity goes **backward** | 1D capacity goes **forward** |

For space-optimized DP:

### 0/1

```cpp
for(int w = W; w >= wt[i]; w--)
```

### Unbounded

```cpp
for(int w = wt[i]; w <= W; w++)
```

This loop direction is a **major interview/OA point**.

---

# Complexity

### Memoization

```text
Time  : O(N × W)
Space : O(N × W)
```

### Tabulation

```text
Time  : O(N × W)
Space : O(N × W)
```

### Space Optimized

```text
Time  : O(N × W)
Space : O(W)
```

---

# Comparison

| Approach | Time | Space |
| -------- | ---- | ----- |
| Recursion | O(2^N) | O(N) |
| Memoization | O(N × W) | O(N × W) |
| Tabulation | O(N × W) | O(N × W) |
| Space Optimized | O(N × W) | **O(W)** |

---

# DP Pattern Learned

```text
2D DP
+
Take / Don't Take
+
Capacity
```

State:

```text
dp[i][w]
```

means:

```text
Maximum value using first i items
with capacity w
```

Choices:

```text
DON'T TAKE
    ↓
dp[i-1][w]
```

```text
TAKE
    ↓
val[i-1] + dp[i-1][w-wt[i-1]]
```

Then:

```text
dp[i][w] =
max(
    don't take,
    take
)
```

---

# Recognition Pattern

When you see:

```text
Items
+
Weight / Cost
+
Capacity / Budget
+
Maximize value / profit
+
Each item can be used at most once
```

Think:

```text
0/1 Knapsack
```

Ask:

```text
1. Can I use each item only once?

2. What is my current item?

3. What capacity is remaining?

4. What happens if I TAKE it?

5. What happens if I DON'T TAKE it?
```

---

# Connection With House Robber

House Robber:

```text
TAKE i
    ↓
nums[i] + dp[i-2]

DON'T TAKE i
    ↓
dp[i-1]
```

0/1 Knapsack:

```text
TAKE item
    ↓
value + previous items with remaining capacity

DON'T TAKE
    ↓
previous items with same capacity
```

Both use:

```text
TAKE / DON'T TAKE
```

But Knapsack adds a second state:

```text
capacity
```

Therefore:

```text
House Robber
     ↓
1D Take / Don't Take

0/1 Knapsack
     ↓
2D Take / Don't Take + Capacity
```

---

# Key Takeaways

- 0/1 Knapsack is one of the most important DP patterns.
- Every item has two choices:
  - Take
  - Don't Take
- Each item can be used **at most once**.
- The state needs both:
  - item index
  - remaining capacity
- Therefore the basic state is 2D:

```text
dp[i][w]
```

- If an item is taken, move to the previous item.
- In space-optimized 0/1 Knapsack, iterate capacity **backwards**.
- Backward iteration prevents using the same item multiple times.

The core pattern is:

```text
                    ITEM i
                   /      \
                TAKE     DON'T TAKE
                 |            |
        value + previous    previous
        with less capacity  with same capacity
                 \            /
                  \          /
                    MAX
                     ↓
                  dp[i][w]
```
