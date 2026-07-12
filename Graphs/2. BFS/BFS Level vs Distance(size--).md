# BFS Level vs Distance

This is one of the most important BFS concepts.

Before writing BFS, always ask:

> **What does one BFS level represent?**

---

# Case 1 — BFS Level Represents Time / Minute

Examples

- Rotting Oranges
- Burn Tree
- Infection Spread
- Level Order Traversal

Here,

```text
One BFS Level

↓

One Minute
```

We must process **all nodes of the current level together**.

Example

```text
Minute 0

2 1 1
1 2 1
```

Queue

```text
(0,0)

(1,1)
```

Both oranges spread **simultaneously**.

So we write

```cpp
while(!q.empty())
{
    int size = q.size();

    while(size--)
    {
        ...
    }

    minutes++;
}
```

✅ Here `size` is **required**.

---

# Case 2 — BFS Already Stores Distance

Examples

- 01 Matrix
- Shortest Path in Binary Matrix
- Word Ladder (distance stored)
- Dijkstra

Each node already knows its answer.

Example

```cpp
dist[row][col]
```

Current

```text
Distance = 3
```

Neighbour

```cpp
dist[newRow][newCol] = dist[row][col] + 1;
```

Since every node already stores its own distance,

we do **not** care about BFS levels.

Simply write

```cpp
while(!q.empty())
{
    auto curr = q.front();
    q.pop();

    ...
}
```

❌ `size` is **not needed**.

---

# Two Ways to Compute Distance

## Method 1 (Recommended)

Store distance in

```cpp
dist[][]
```

Example

```cpp
dist[newRow][newCol] = dist[row][col] + 1;
```

No `size` required.

---

## Method 2

Do not use `dist[][]`.

Instead use

```cpp
steps
```

Every BFS level becomes

```text
Distance + 1
```

Now `size` is required.

---

# Comparison

| Problem | Need `size`? | Why |
|----------|-------------:|-----|
| Rotting Oranges | ✅ Yes | One level = One minute |
| Level Order Traversal | ✅ Yes | One level = One tree level |
| Burn Tree | ✅ Yes | One level = One second |
| 01 Matrix | ❌ No | `dist[][]` stores distance |
| Shortest Path Binary Matrix | ❌ No | `dist[][]` stores distance |
| Dijkstra | ❌ No | Priority Queue + Distance Array |

---

# Interview Rule

```text
Need Time / Minute / Level?

↓

YES

↓

Use

int size = q.size();

while(size--)
```

---

```text
Distance already stored?

↓

YES

↓

No size needed.
```

---

# Key Takeaways

- **Use `size` when one BFS level has a real meaning** (minute, second, tree level, wave).
- **Do not use `size` if every node already stores its own distance.**
- `dist[][]` replaces the need for counting BFS levels.
- Before writing BFS, always ask:
  - **Am I counting levels?**
  - **Or does each node already know its distance?**
