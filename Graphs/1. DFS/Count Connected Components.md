# Count Connected Components

| Topic | Difficulty | Pattern                    |
| ----- | ---------- | -------------------------- |
| Graph | Medium     | DFS + Connected Components |

---

# 📌 Problem Description

Given an **undirected graph** with **V** vertices numbered from **0** to **V-1** and an edge list **edges[][]**, count the number of **connected components**.

A connected component is a group of vertices where every vertex is reachable from every other vertex in that group.

Return the total number of connected components.

Example 1

```text
Input

V = 5

edges =
[
 [0,1],
 [2,1],
 [3,4]
]

Output

2
```

Graph

```text
0 ----- 1 ----- 2


3 ----- 4
```

Components

```text
{0,1,2}

{3,4}
```

---

Example 2

```text
Input

V = 7

edges =
[
 [0,1],
 [6,0],
 [2,4],
 [2,3],
 [3,4]
]

Output

3
```

Graph

```text
6
|
0 ----- 1


2 ----- 3
 \     /
   \ /
    4


5
```

Components

```text
{0,1,6}

{2,3,4}

{5}
```

---

# 🧠 Intuition

If we start DFS from any node,

DFS visits **every node that belongs to the same connected component**.

Therefore,

```text
New DFS Call

↓

New Connected Component
```

Every time we find an **unvisited node**, we have discovered a completely new component.

---

# Key Observation

Input is **not** an adjacency list.

It is an **edge list**.

```text
edges

↓

Build Adjacency List

↓

Run DFS
```

---

# Step 1

Build Graph

```cpp
vector<vector<int>> adj(V);
```

For every edge

```text
u ----- v
```

store

```cpp
adj[u].push_back(v);

adj[v].push_back(u);
```

Example

```text
edges

0 1

2 1

3 4
```

Adjacency List

```text
adj[0] = [1]

adj[1] = [0,2]

adj[2] = [1]

adj[3] = [4]

adj[4] = [3]
```

---

# Step 2

Create Visited Array

```cpp
vector<int> visited(V,0);
```

Meaning

```text
0

↓

Not Visited


1

↓

Already Visited
```

---

# Step 3

Run DFS

Loop through every node.

```text
If node is not visited

↓

Run DFS

↓

DFS visits the entire connected component

↓

components++
```

---

# DFS Logic

```text
Current Node

↓

Mark Visited

↓

Visit Every Neighbour

↓

If Neighbour Not Visited

↓

DFS(Neighbour)
```

Memory Trick

```text
MARK

↓

LOOP

↓

RECURSE
```

---

# Algorithm

```text
Edge List

↓

Build Adjacency List

↓

Visited Array

↓

Loop Through Every Node

↓

Unvisited Node ?

↓

DFS

↓

Component++

↓

Return Components
```

---

# Code

```cpp
class Solution {
public:

    void dfs(int node,
             vector<vector<int>>& adj,
             vector<int>& visited)
    {
        visited[node] = 1;

        for(int neighbour : adj[node])
        {
            if(visited[neighbour] == 0)
            {
                dfs(neighbour, adj, visited);
            }
        }
    }

    int countConnected(int V, vector<vector<int>>& edges)
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

        int components = 0;

        for(int node = 0; node < V; node++)
        {
            if(visited[node] == 0)
            {
                dfs(node, adj, visited);
                components++;
            }
        }

        return components;
    }
};
```

---

# One Dry Run

Input

```text
V = 5

edges

0 1

2 1

3 4
```

Adjacency List

```text
0 -> 1

1 -> 0 2

2 -> 1

3 -> 4

4 -> 3
```

Initial

```text
visited

0 0 0 0 0

components = 0
```

Start Loop

```text
Node = 0

↓

DFS(0)

↓

Visits

0 → 1 → 2

↓

visited

1 1 1 0 0

↓

components = 1
```

Continue Loop

```text
Node 1

↓

Already Visited
```

```text
Node 2

↓

Already Visited
```

```text
Node 3

↓

DFS(3)

↓

Visits

3 → 4

↓

visited

1 1 1 1 1

↓

components = 2
```

Return

```text
2
```

---

# Common Mistakes

### ❌ Forgetting to Build Adjacency List

Input is an edge list, not an adjacency list.

---

### ❌ Forgetting `visited`

Causes infinite recursion.

```text
0

↓

1

↓

0

↓

1
```

---

### ❌ Using Only

```cpp
dfs(0);
```

Fails for disconnected graphs.

Always use the outer loop.

---

### ❌ Incrementing Components Inside DFS

Wrong.

Increment only when starting a **new DFS** from the outer loop.

---

# Complexity

| Complexity      | Value        |
| --------------- | ------------ |
| Time            | **O(V + E)** |
| Space           | **O(V + E)** |
| Auxiliary Space | **O(V)**     |

* Building adjacency list: **O(E)**
* DFS traversal: **O(V + E)**
* Visited array + recursion stack: **O(V)**

---

# Key Takeaways

* Every DFS explores **one complete connected component**.
* Every **new DFS call** from the outer loop means a **new component**.
* Build the adjacency list first because the input is an **edge list**.
* The outer loop handles disconnected graphs.
* This is the **base DFS template** for many graph problems.

---

# Similar Problems (Same Pattern)

```text
Connected Components
        │
        ├── Number of Provinces (LC 547)
        ├── Number of Islands (LC 200)
        ├── Flood Fill (LC 733)
        ├── Surrounded Regions (LC 130)
        ├── Number of Enclaves (LC 1020)
        └── Number of Distinct Islands (GFG / LC Premium)
```

Only the graph representation changes:

* Graph → Adjacency List
* Matrix → Adjacency Matrix
* Grid → Directions (Up, Down, Left, Right)

The DFS idea remains the same.
