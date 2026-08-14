# Frog Jump

| Topic | Difficulty | Pattern |
| ----- | ---------- | ------- |
| Dynamic Programming | Medium | 1D DP — Minimum Cost |

---

# 📌 Problem Description

You are given an integer array `height[]`.

```text
height[i] = height of the i-th stair
```

A frog starts at the **first stair** and wants to reach the **last stair**.

From stair `i`, the frog can jump:

```text
1 stair
OR
2 stairs
```

The cost of a jump is the absolute difference between the heights of the two stairs.

```text
Jump Cost = |height[i] - height[j]|
```

Find the **minimum total cost** required to reach the last stair.

---

# Example

### Example 1

Input

```text
height = [20,30,40,20]
```

Output

```text
20
```

Explanation:

Optimal path:

```text
0 → 1 → 3
```

Costs:

```text
0 → 1

|30 - 20| = 10
```

```text
1 → 3

|20 - 30| = 10
```

Total:

```text
10 + 10 = 20
```

---

### Example 2

Input

```text
height = [30,20,50,10,40]
```

Output

```text
30
```

Optimal path:

```text
0 → 2 → 4
```

Costs:

```text
0 → 2

|50 - 30| = 20
```

```text
2 → 4

|40 - 50| = 10
```

Total:

```text
20 + 10 = 30
```

---

# 🧠 Intuition

This is another **1D DP** problem.

At stair `i`, the frog can arrive from only two places:

```text
i - 1
```

or

```text
i - 2
```

Therefore, there are two possible complete routes to reach `i`.

---

# Key Observation

Define:

```text
dp[i] = minimum cost required to reach stair i
```

To reach stair `i`, there are two choices.

### Choice 1 — Jump from `i-1`

The previous cost is:

```text
dp[i-1]
```

Jump cost:

```text
|height[i] - height[i-1]|
```

Complete cost:

```text
dp[i-1] + |height[i] - height[i-1]|
```

---

### Choice 2 — Jump from `i-2`

The previous cost is:

```text
dp[i-2]
```

Jump cost:

```text
|height[i] - height[i-2]|
```

Complete cost:

```text
dp[i-2] + |height[i] - height[i-2]|
```

---

Therefore:

```text
dp[i] = min(
    complete cost from i-1,
    complete cost from i-2
)
```

---

# ⚠️ Important DP Lesson

Do **not** do this:

```cpp
min(jumpCost1, jumpCost2) + min(dp[i-1], dp[i-2])
```

This is wrong because the minimum jump cost and minimum previous cost can belong to **different routes**.

For example:

```text
dp[i-1] = 100
dp[i-2] = 5

jump(i-1 → i) = 1
jump(i-2 → i) = 50
```

Two complete routes are:

```text
Route 1:

100 + 1 = 101
```

```text
Route 2:

5 + 50 = 55
```

Correct:

```text
min(101,55) = 55
```

But the wrong approach gives:

```text
min(100,5) + min(1,50)

= 5 + 1

= 6
```

`6` is impossible because:

```text
5
```

belongs to the route through `i-2`, while:

```text
1
```

belongs to the route through `i-1`.

---

# Correct DP Rule

When a choice contains multiple dependent parts:

```text
Previous Answer
+
Transition Cost
```

calculate the **complete candidate first**.

Then take `min` or `max`.

Correct structure:

```text
             min
            /   \
           /     \
      Route 1   Route 2
         |         |
      dp[i-1]    dp[i-2]
         +         +
      jump 1     jump 2
```

---

# State

```text
dp[i] = minimum cost required to reach stair i
```

Base case:

```text
dp[0] = 0
```

We are already standing on stair `0`, so the cost to reach it is `0`.

For stair `1`:

```text
dp[1] = |height[1] - height[0]|
```

---

# Algorithm

```text
Create dp array

↓

dp[0] = 0

↓

dp[1] = |height[1] - height[0]|

↓

For i = 2 to n-1

    oneStep =
        dp[i-1] +
        |height[i] - height[i-1]|

    twoStep =
        dp[i-2] +
        |height[i] - height[i-2]|

    dp[i] = min(oneStep, twoStep)

↓

Return dp[n-1]
```

---

# Code — Tabulation

```cpp
class Solution {
public:

    int minCost(vector<int>& height)
    {
        int n = height.size();

        vector<int> dp(n, 0);

        dp[0] = 0;

        dp[1] = abs(height[1] - height[0]);

        for(int i = 2; i < n; i++)
        {
            int oneStep =
                dp[i-1] +
                abs(height[i] - height[i-1]);

            int twoStep =
                dp[i-2] +
                abs(height[i] - height[i-2]);

            dp[i] = min(oneStep, twoStep);
        }

        return dp[n-1];
    }
};
```

---

# Dry Run

Consider:

```text
height = [30,20,50,10,40]
```

Initial:

```text
dp[0] = 0
```

For stair `1`:

```text
dp[1] = |20 - 30|

      = 10
```

So:

```text
dp = [0,10,...]
```

---

### i = 2

One-step jump:

```text
0 → 1 → 2
```

Cost:

```text
dp[1] + |50 - 20|

= 10 + 30

= 40
```

Two-step jump:

```text
0 → 2
```

Cost:

```text
dp[0] + |50 - 30|

= 0 + 20

= 20
```

Therefore:

```text
dp[2] = min(40,20)

      = 20
```

---

### i = 3

One-step:

```text
dp[2] + |10 - 50|

= 20 + 40

= 60
```

Two-step:

```text
dp[1] + |10 - 20|

= 10 + 10

= 20
```

Therefore:

```text
dp[3] = 20
```

---

### i = 4

One-step:

```text
dp[3] + |40 - 10|

= 20 + 30

= 50
```

Two-step:

```text
dp[2] + |40 - 50|

= 20 + 10

= 30
```

Therefore:

```text
dp[4] = 30
```

Final answer:

```text
30
```

---

# Code — Memoization

```cpp
class Solution {
public:

    int solve(int i, vector<int>& height, vector<int>& dp)
    {
        if(i == 0)
            return 0;

        if(dp[i] != -1)
            return dp[i];

        int oneStep =
            solve(i-1, height, dp) +
            abs(height[i] - height[i-1]);

        int twoStep = INT_MAX;

        if(i > 1)
        {
            twoStep =
                solve(i-2, height, dp) +
                abs(height[i] - height[i-2]);
        }

        return dp[i] = min(oneStep, twoStep);
    }

    int minCost(vector<int>& height)
    {
        int n = height.size();

        vector<int> dp(n, -1);

        return solve(n-1, height, dp);
    }
};
```

---

# Complexity — Memoization

| Complexity | Value |
|------------|-------|
| Time | **O(n)** |
| Space | **O(n)** |

Each state is calculated only once.

Space includes:

```text
DP array → O(n)
Recursion stack → O(n)
```

---

# Space Optimization

Notice:

```text
dp[i]
```

only depends on:

```text
dp[i-1]
dp[i-2]
```

Therefore, we don't need the entire DP array.

Keep only:

```text
first  = dp[i-2]
second = dp[i-1]
```

Then calculate:

```text
current =
    min(
        second + |height[i] - height[i-1]|,
        first + |height[i] - height[i-2]|
    )
```

Then shift:

```text
first = second
second = current
```

---

# Code — Space Optimized

```cpp
class Solution {
public:

    int minCost(vector<int>& height)
    {
        int n = height.size();

        int first = 0;

        int second =
            abs(height[1] - height[0]);

        for(int i = 2; i < n; i++)
        {
            int oneStep =
                second +
                abs(height[i] - height[i-1]);

            int twoStep =
                first +
                abs(height[i] - height[i-2]);

            int current = min(oneStep, twoStep);

            first = second;
            second = current;
        }

        return second;
    }
};
```

---

# Complexity — Space Optimized

| Complexity | Value |
|------------|-------|
| Time | **O(n)** |
| Space | **O(1)** |

Only three variables are used:

```text
first
second
current
```

---

# Comparison With Previous Problems

### Climbing Stairs

```text
dp[i] = dp[i-1] + dp[i-2]
```

Because we are **counting ways**.

### Min Cost Climbing Stairs

```text
dp[i] =
cost[i] +
min(dp[i-1], dp[i-2])
```

Because we are **minimizing cost**.

### Frog Jump

```text
dp[i] = min(
    dp[i-1] + jumpCost1,
    dp[i-2] + jumpCost2
)
```

Because each choice has a **different transition cost**.

---

# DP Pattern Learned

```text
1D DP
```

State:

```text
dp[i] = minimum cost to reach i
```

Choices:

```text
i-1 → i
i-2 → i
```

Transitions:

```text
dp[i-1] + |height[i] - height[i-1]|

dp[i-2] + |height[i] - height[i-2]|
```

Final:

```text
dp[n-1]
```

---

# Recognition Pattern

When you see:

```text
1D array
+
Current position
+
Can move a fixed number of steps
+
Each move has a cost
+
Need minimum / maximum total cost
```

Think:

```text
1D DP
```

Then ask:

```text
1. What does dp[i] mean?

2. Which previous states can reach i?

3. What is the transition cost from each previous state?

4. Calculate each complete candidate.

5. Take min/max between candidates.
```

---

# Key Takeaways

- This is a **1D DP** problem.
- `dp[i]` represents the minimum cost to reach stair `i`.
- The frog can reach `i` from `i-1` or `i-2`.
- Each jump has its own cost.
- Calculate the **complete cost of each route** before taking `min`.
- Do not do:

```text
min(previous states) + min(jump costs)
```

- Instead do:

```text
min(
    previous state 1 + its transition cost,
    previous state 2 + its transition cost
)
```

- Tabulation uses **O(n)** space.
- Space optimization uses **O(1)** space.
- The main DP lesson is:

```text
CHOICE 1
    ↓
complete candidate

CHOICE 2
    ↓
complete candidate

       ↓

    min / max
```
