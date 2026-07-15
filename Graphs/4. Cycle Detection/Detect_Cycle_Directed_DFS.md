# Detect Cycle in Directed Graph (DFS)

| Topic | Difficulty | Pattern |
|--------|------------|---------|
| Graph | Medium | DFS + Recursion Stack |

---

# 📌 Problem

Given a **Directed Graph**, determine whether it contains a cycle.

Unlike an undirected graph, **parent tracking does not work**.

Instead, use

- `visited[]`
- `pathVisited[]` (Current DFS Stack)

---

# 🧠 Intuition

Think of `pathVisited[]` as

```text
Current DFS Recursion Stack
```

If during DFS you reach a node that is **already in the current recursion stack**, then you have returned to an ancestor.

That forms a **cycle**.

---

# Why visited[] is NOT Enough

Example

```text
0 → 1 → 2

3 → 1
```

DFS from

```text
0
```

visits

```text
0

↓

1

↓

2
```

Later,

DFS starts from

```text
3
```

and reaches

```text
1
```

Node

```text
1
```

is already visited.

But this is **NOT** a cycle.

Why?

Because

```text
1
```

is no longer in the current DFS path.

Hence we need another array.

---

# Meaning of Arrays

```cpp
visited[node]
```

Means

```text
Has this node ever been visited?
```

---

```cpp
pathVisited[node]
```

Means

```text
Is this node currently inside the recursion stack?
```

---

# Algorithm

```text
For every unvisited node

↓

DFS(node)

↓

visited[node] = 1

pathVisited[node] = 1

↓

Visit every neighbour

↓

Neighbour not visited ?

↓

DFS(neighbour)

↓

DFS returned true ?

↓

Return true

↓

Neighbour already visited
AND

pathVisited[neighbour] == 1 ?

↓

Cycle Found

↓

Return true

↓

Finished all neighbours

↓

pathVisited[node] = 0

↓

Return false
```

---

# Code

```cpp
class Solution {
public:

    bool dfs(int node,
             vector<int>& visited,
             vector<vector<int>>& adj,
             vector<int>& pathVisited)
    {
        visited[node] = 1;
        pathVisited[node] = 1;

        for(int neighbour : adj[node])
        {
            if(!visited[neighbour])
            {
                if(dfs(neighbour, visited, adj, pathVisited))
                    return true;
            }
            else if(pathVisited[neighbour])
            {
                return true;
            }
        }

        pathVisited[node] = 0;

        return false;
    }

    bool isCyclic(int V, vector<vector<int>>& edges)
    {
        vector<vector<int>> adj(V);

        for(auto edge : edges)
        {
            int u = edge[0];
            int v = edge[1];

            adj[u].push_back(v);
        }

        vector<int> visited(V,0);
        vector<int> pathVisited(V,0);

        for(int i=0;i<V;i++)
        {
            if(!visited[i])
            {
                if(dfs(i, visited, adj, pathVisited))
                    return true;
            }
        }

        return false;
    }
};
```

---

# Dry Run

Graph

```text
0 → 1

1 → 2

2 → 3

3 → 1
```

Start DFS

```text
0
```

Current Stack

```text
0
```

---

Go to

```text
1
```

Current Stack

```text
0

↓

1
```

---

Go to

```text
2
```

Current Stack

```text
0

↓

1

↓

2
```

---

Go to

```text
3
```

Current Stack

```text
0

↓

1

↓

2

↓

3
```

---

Neighbour of

```text
3
```

is

```text
1
```

Check

```cpp
visited[1] == 1
```

Yes.

Check

```cpp
pathVisited[1] == 1
```

Yes.

This means

```text
1 → 2 → 3 → 1
```

Cycle found.

Return

```text
true
```

---

# Why pathVisited[node] = 0 ?

Suppose

```text
0 → 1 → 2
```

No cycle.

After finishing

```text
2
```

remove it from recursion stack.

Then remove

```text
1
```

Then remove

```text
0
```

Finally

```text
pathVisited

0 0 0
```

No node remains in the current DFS path.

---

# Common Mistakes

## ❌ Using parent

Parent is only for

```text
Undirected Graph
```

Directed graphs require

```text
pathVisited[]
```

---

## ❌ Forgetting

```cpp
pathVisited[node] = 0;
```

Then nodes wrongly remain in the recursion stack and false cycles are detected.

---

## ❌ Checking only visited[]

Wrong.

A visited node is **not necessarily** part of a cycle.

Only

```cpp
visited[node] && pathVisited[node]
```

indicates a back edge.

---

# Complexity

| Complexity | Value |
|------------|-------|
| Time | **O(V + E)** |
| Space | **O(V)** |

---

# DFS vs Kahn's Algorithm

| DFS | Kahn's Algorithm |
|------|------------------|
| Uses `visited[]` + `pathVisited[]` | Uses `indegree[]` |
| Detects back edge | Detects unprocessed nodes |
| Recursive | BFS |
| No queue | Queue |

---

# Recognition Pattern ⭐⭐⭐⭐⭐

Whenever the problem asks

```text
Directed Graph

+

Cycle Detection
```

Think of two approaches

1. DFS + `visited[]` + `pathVisited[]`
2. Kahn's Algorithm (Topological Sort)

---

# Key Takeaways

- Parent is **not** used in directed graphs.
- `visited[]` tells whether a node was ever visited.
- `pathVisited[]` tells whether a node is currently in the DFS recursion stack.
- A cycle exists if we reach a node where

```cpp
visited[node] == 1

&&

pathVisited[node] == 1
```

- Before returning from DFS, always remove the node from the recursion stack using

```cpp
pathVisited[node] = 0;
```
