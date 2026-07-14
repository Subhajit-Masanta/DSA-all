# Topological Sort (Kahn's Algorithm)

| Topic | Difficulty | Pattern |
|--------|------------|---------|
| Graph | Medium | BFS + Indegree |

---

# 📌 Problem Description

Given a **Directed Acyclic Graph (DAG)**, return a valid **Topological Ordering**.

A Topological Order is a linear ordering of vertices such that

```text
u → v
```

means

```text
u must appear before v
```

There may be multiple correct answers.

---

# Example

Edges

```text
4 → 1

4 → 0

5 → 0

5 → 2

2 → 3

1 → 3
```

Graph

```text
      4        5
     / \      / \
    1   0    2   0
     \      /
      \    /
        3
```

One valid answer

```text
4 5 1 2 0 3
```

Another

```text
5 4 2 1 0 3
```

Both are correct.

---

# 🧠 Intuition

Think of every edge as a dependency.

```text
4 → 1
```

means

```text
Finish 4 first

↓

Then do 1
```

Question becomes

```text
Which nodes have no remaining dependencies?
```

Those nodes can be processed immediately.

---

# Indegree

Indegree means

```text
Number of incoming edges
```

or

```text
Remaining Dependencies
```

Example

```text
4 → 1

5 → 2

1 → 3

2 → 3
```

Indegree

| Node | Indegree |
|------|---------:|
| 0 | 0 |
| 1 | 1 |
| 2 | 1 |
| 3 | 2 |
| 4 | 0 |
| 5 | 0 |

Nodes

```text
4

5
```

have indegree

```text
0
```

So they can be processed first.

---

# Key Idea

Every time we process a node,

its outgoing edges disappear.

Example

```text
4 → 1

4 → 0
```

Process

```text
4
```

Remove both edges.

Now

```text
indegree[1]--

indegree[0]--
```

If any node becomes

```text
0
```

it is ready.

Push it into the queue.

---

# Algorithm

```text
Build Adjacency List

↓

Compute Indegree

↓

Push every node with indegree = 0

↓

While Queue not Empty

↓

Pop Node

↓

Store in Answer

↓

Visit every neighbour

↓

indegree[neighbour]--

↓

If indegree becomes 0

↓

Push neighbour

↓

Repeat

↓

Return Answer
```

---

# Code

```cpp
class Solution {
public:

    vector<int> topoSort(int V, vector<vector<int>>& edges)
    {
        vector<int> indegree(V,0);

        vector<vector<int>> adj(V);

        for(auto edge : edges)
        {
            int u = edge[0];
            int v = edge[1];

            adj[u].push_back(v);

            indegree[v]++;
        }

        queue<int> q;

        for(int i=0;i<V;i++)
        {
            if(indegree[i]==0)
                q.push(i);
        }

        vector<int> ans;

        while(!q.empty())
        {
            int node=q.front();
            q.pop();

            ans.push_back(node);

            for(int neighbour : adj[node])
            {
                indegree[neighbour]--;

                if(indegree[neighbour]==0)
                    q.push(neighbour);
            }
        }

        return ans;
    }
};
```

---

# Dry Run

Graph

```text
4 → 1

4 → 0

5 → 0

5 → 2

2 → 3

1 → 3
```

Initial Indegree

```text
0 : 2

1 : 1

2 : 1

3 : 2

4 : 0

5 : 0
```

Queue

```text
4

5
```

---

Pop

```text
4
```

Answer

```text
4
```

Decrease

```text
indegree[1]--

indegree[0]--
```

Now

```text
1 → 0
```

Push

```text
1
```

Queue

```text
5

1
```

---

Pop

```text
5
```

Answer

```text
4 5
```

Decrease

```text
indegree[0]--

indegree[2]--
```

Now

```text
0 → 0

2 → 0
```

Push

```text
0

2
```

Queue

```text
1

0

2
```

Continue until queue becomes empty.

One valid order

```text
4 5 1 0 2 3
```

---

# Why BFS?

Queue always stores

```text
Nodes whose dependencies are finished.
```

As soon as a node's indegree becomes

```text
0
```

it becomes eligible for processing.

---

# Common Mistakes

## ❌ Making graph undirected

Wrong

```cpp
adj[v].push_back(u);
```

Topological Sort works only on **Directed Graphs**.

---

## ❌ Decreasing indegree of current node

Wrong

```cpp
indegree[node]--;
```

Correct

```cpp
indegree[neighbour]--;
```

Only neighbours lose one dependency.

---

## ❌ Traversing edge list instead of adjacency list

Wrong

```cpp
edges[node]
```

Correct

```cpp
adj[node]
```

---

## ❌ Forgetting to push nodes whose indegree becomes 0

They are now ready to be processed.

---

# Complexity

| Complexity | Value |
|------------|-------|
| Time | **O(V + E)** |
| Auxiliary Space | **O(V + E)** |

### Why?

- Building adjacency list → **O(E)**
- Computing indegree → **O(E)**
- Every node enters the queue at most once → **O(V)**
- Every edge is processed exactly once → **O(E)**

Total

```text
O(V + E)
```

---

# Kahn's Algorithm Summary

```text
Find nodes with indegree 0

↓

Push into Queue

↓

Pop

↓

Answer

↓

Remove outgoing edges

↓

Decrease neighbour indegree

↓

Neighbour becomes indegree 0?

↓

Push

↓

Repeat
```

---

# Recognition Pattern ⭐⭐⭐⭐⭐

Whenever a problem mentions

```text
Before

After

Dependency

Prerequisite

Schedule

Build Order

Compilation Order
```

Think

```text
Directed Graph

↓

Topological Sort

↓

Kahn's Algorithm
```

---

# Key Takeaways

- Works only on **Directed Acyclic Graphs (DAGs)**.
- `indegree[node]` = number of remaining prerequisites.
- Queue stores nodes with **indegree = 0**.
- Process a node, remove its outgoing edges, and update neighbours.
- If a neighbour's indegree becomes **0**, push it into the queue.
- Multiple valid topological orders may exist.
