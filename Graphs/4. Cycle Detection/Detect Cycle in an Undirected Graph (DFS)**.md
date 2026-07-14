# Detect Cycle in an Undirected Graph (DFS)

| Topic | Difficulty | Pattern |
|--------|------------|---------|
| Graph | Medium | DFS + Parent Tracking |

---

# 📌 Problem Description

Given an **undirected graph**, determine whether it contains a cycle.

The graph may have **multiple connected components**.

Return

```text
true
```

if a cycle exists.

Otherwise

```text
false
```

---

## Input

```cpp
V = 4

edges = [[0,1],[0,2],[1,2],[2,3]]
```

Build adjacency list

```text
0 → 1 2

1 → 0 2

2 → 0 1 3

3 → 2
```

Graph

```text
      0
     / \
    1---2
         \
          3
```

Output

```text
true
```

---

# Example 2

```cpp
V = 4

edges = [[0,1],[1,2],[2,3]]
```

Graph

```text
0 ----- 1 ----- 2 ----- 3
```

Output

```text
false
```

---

# 🧠 Intuition

Normally in DFS

if we find

```text
Visited Neighbour
```

we may think

```text
Cycle
```

But in an **undirected graph**

every edge appears twice.

Example

```text
0 ----- 1
```

Adjacency List

```text
0 → 1

1 → 0
```

During DFS

```text
0

↓

1
```

At node

```text
1
```

we again see

```text
0
```

already visited.

This is **NOT** a cycle.

It is simply the node we came from.

So we need to remember

```text
Parent
```

---

# Key Observation

For every neighbour

There are only three cases.

### Case 1

Neighbour is not visited

↓

Continue DFS.

---

### Case 2

Neighbour is visited

AND

Neighbour is Parent

↓

Ignore.

---

### Case 3

Neighbour is visited

AND

Neighbour is NOT Parent

↓

Cycle Found.

---

# Algorithm

```text
Build Adjacency List

↓

Visited Array

↓

For every Component

↓

DFS(Node, Parent)

↓

Neighbour Not Visited

↓

DFS

↓

Neighbour == Parent

↓

Ignore

↓

Visited Neighbour != Parent

↓

Cycle Found

↓

Return true

↓

Finished all Components

↓

Return false
```

---

# DFS Code

```cpp
class Solution {
public:

    bool dfs(int node,
             int parent,
             vector<int>& visited,
             vector<vector<int>>& adj)
    {
        visited[node] = 1;

        for(int neighbour : adj[node])
        {
            if(!visited[neighbour])
            {
                if(dfs(neighbour, node, visited, adj))
                    return true;
            }
            else if(neighbour == parent)
            {
                continue;
            }
            else
            {
                return true;
            }
        }

        return false;
    }

    bool isCycle(int V, vector<vector<int>>& edges)
    {
        vector<vector<int>> adj(V);

        for(auto edge : edges)
        {
            int u = edge[0];
            int v = edge[1];

            adj[u].push_back(v);
            adj[v].push_back(u);
        }

        vector<int> visited(V,0);

        for(int i=0;i<V;i++)
        {
            if(!visited[i])
            {
                if(dfs(i,-1,visited,adj))
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
      0
     / \
    1---2
         \
          3
```

Start

```text
dfs(0,-1)
```

Visit

```text
0
```

↓

Visit

```text
1
```

↓

Neighbour

```text
0
```

Parent

↓

Ignore

↓

Visit

```text
2
```

Neighbours

```text
1
```

Parent

↓

Ignore

Next neighbour

```text
0
```

Already visited

AND

Not Parent

↓

Cycle Found

Return

```text
true
```

---

# Why Parent?

Without Parent

Example

```text
0 ----- 1
```

DFS

```text
0

↓

1
```

At

```text
1
```

Neighbour

```text
0
```

Already visited.

Without checking Parent,

you would wrongly return

```text
true
```

for every graph.

---

# Common Mistakes

## ❌ Forgetting Parent

Every graph becomes a cycle.

---

## ❌ Returning DFS immediately

Wrong

```cpp
if(!visited[neighbour])
    return dfs(...);
```

Correct

```cpp
if(!visited[neighbour])
{
    if(dfs(...))
        return true;
}
```

Continue checking other neighbours if DFS returns false.

---

## ❌ Returning after first component

Wrong

```cpp
for(...)
{
    return dfs(...);
}
```

Graph may have multiple components.

Always check every component.

---

## ❌ Forgetting Root Parent

Start

```cpp
dfs(start,-1);
```

Root has no parent.

---

# Complexity

| Complexity | Value |
|------------|-------|
| Time | **O(V + E)** |
| Auxiliary Space | **O(V)** |

### Why?

- Every vertex is visited once.
- Every edge is checked once.
- `visited[]` uses **O(V)**.
- DFS recursion stack is **O(V)** in the worst case.

---

# Key Takeaways

- Parent tracking is the main idea.
- A visited neighbour is **not always** a cycle.
- Ignore the edge leading back to the parent.
- If a visited neighbour is **not** the parent, a cycle exists.
- Always check all connected components.
