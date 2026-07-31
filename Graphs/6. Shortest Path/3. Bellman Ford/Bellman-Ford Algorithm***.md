# Bellman-Ford Algorithm

| Topic | Difficulty | Pattern |
|--------|------------|---------|
| Graph | Medium | Shortest Path + Edge Relaxation |

---

# 📌 Problem Description

You are given a weighted directed graph.

Each edge is represented as

```cpp
[u, v, wt]
```

meaning

```text
u ----wt----> v
```

You are also given

```cpp
src
```

Find the shortest distance from

```text
src
```

to every vertex.

If a vertex is unreachable,

store

```text
1e8
```

If a **negative weight cycle** is reachable from the source,

return

```text
{-1}
```

Bellman-Ford works even when edge weights are negative and can detect reachable negative-weight cycles. :contentReference[oaicite:0]{index=0}

---

# Example 1

Input

```text
V = 5

Edges

0 → 1 (5)

1 → 2 (1)

2 → 4 (1)

4 → 3 (-1)

1 → 3 (2)
```

Source

```text
0
```

Output

```text
0 5 6 6 7
```

---

# Example 2

Input

```text
1 → 2 (-6)

2 → 3 (5)

3 → 1 (-2)
```

Output

```text
-1
```

Because

```text
Cycle Weight

-6 + 5 + (-2)

=

-3
```

Negative cycle exists.

---

# 🧠 Intuition

Dijkstra works only when

```text
All edge weights

≥ 0
```

Bellman-Ford removes this restriction.

Instead of choosing the minimum node,

it repeatedly relaxes

```text
Every Edge
```

until all shortest paths are found.

---

# Key Observation

Suppose

```text
0 → 1 → 2 → 3
```

Initially

```text
0 INF INF INF
```

Pass 1

```text
0 5 INF INF
```

Pass 2

```text
0 5 8 INF
```

Pass 3

```text
0 5 8 10
```

Each pass allows shortest paths using

```text
One More Edge
```

Since a shortest simple path can contain at most

```text
V-1

Edges
```

we relax all edges exactly

```text
V-1

Times
```

If after those passes an edge can still be relaxed,

then a reachable negative-weight cycle exists. :contentReference[oaicite:1]{index=1}

---

# Relaxation

Exactly the same as Dijkstra.

```cpp
if(dist[u] != INF &&
   dist[u] + wt < dist[v])
{
    dist[v] = dist[u] + wt;
}
```

---

# Algorithm

```text
Distance Array = INF

↓

dist[src] = 0

↓

Repeat V−1 Times

↓

Traverse Every Edge

↓

Relax Edge

↓

Repeat

↓

One More Pass

↓

Can Any Edge Relax?

↓

Yes

↓

Negative Cycle

↓

Return {-1}

↓

Otherwise

↓

Replace INF by 1e8

↓

Return Distance Array
```

---

# Code

```cpp
class Solution {
public:

    vector<int> bellmanFord(int V,
                            vector<vector<int>>& edges,
                            int src)
    {
        vector<int> dist(V, INT_MAX);

        dist[src] = 0;

        for(int i=1;i<=V-1;i++)
        {
            for(auto edge : edges)
            {
                int u = edge[0];
                int v = edge[1];
                int wt = edge[2];

                if(dist[u] != INT_MAX &&
                   dist[u] + wt < dist[v])
                {
                    dist[v] = dist[u] + wt;
                }
            }
        }

        for(auto edge : edges)
        {
            int u = edge[0];
            int v = edge[1];
            int wt = edge[2];

            if(dist[u] != INT_MAX &&
               dist[u] + wt < dist[v])
            {
                return {-1};
            }
        }

        for(int i=0;i<V;i++)
        {
            if(dist[i] == INT_MAX)
                dist[i] = 1e8;
        }

        return dist;
    }
};
```

---

# Dry Run

Graph

```text
0 →1 (5)

1 →2 (3)

2 →3 (2)
```

Initially

```text
dist

0 INF INF INF
```

---

Pass 1

Relax

```text
0→1
```

```text
dist

0 5 INF INF
```

Relax

```text
1→2
```

```text
dist

0 5 8 INF
```

Relax

```text
2→3
```

```text
dist

0 5 8 10
```

---

Pass 2

No updates.

---

Pass 3

No updates.

---

Negative Cycle Check

No edge relaxes.

Return

```text
0 5 8 10
```

---

# Why Bellman-Ford Works?

After the first pass,

all shortest paths using at most

```text
1 Edge
```

are correct.

After the second pass,

all shortest paths using at most

```text
2 Edges
```

are correct.

...

After

```text
V−1

Passes
```

all shortest simple paths are correct because a simple path cannot contain more than `V−1` edges. If an additional pass can still reduce a distance, the only explanation is that a reachable negative-weight cycle was used. :contentReference[oaicite:2]{index=2}

---

# Common Mistakes

## ❌ Relaxing Only Once

Wrong

```cpp
for(edge)
```

Correct

```cpp
for(V-1 times)
{
    for(edge)
}
```

---

## ❌ Forgetting Reachability Check

Wrong

```cpp
if(dist[u]+wt<dist[v])
```

Correct

```cpp
if(dist[u]!=INT_MAX &&
   dist[u]+wt<dist[v])
```

---

## ❌ Forgetting Negative Cycle Check

Always perform

```text
One Extra Pass
```

after

```text
V−1

Iterations
```

---

## ❌ Returning INT_MAX

Problem requires

```text
1e8
```

for unreachable vertices.

---

## ❌ Using Priority Queue

Bellman-Ford

```text
Does NOT

Use Priority Queue
```

---

# Complexity

| Complexity | Value |
|------------|-------|
| Time | **O(V × E)** |
| Auxiliary Space | **O(V)** |

### Why?

- Every iteration scans all edges.
- There are

```text
V−1
```

iterations.

- Distance array stores one value per vertex.

---

# Key Takeaways

- Bellman-Ford works with **negative edge weights**.
- It repeatedly relaxes **every edge**.
- Relax all edges exactly

```text
V−1
```

times.
- Perform **one extra pass** to detect a reachable negative cycle.
- If any edge still relaxes,

```cpp
return {-1};
```

- No priority queue or adjacency list is required.
- Time Complexity

```text
O(V × E)
```

- Space Complexity

```text
O(V)
```
