# Topological Sort (DFS)

| Topic | Difficulty | Pattern |
|--------|------------|---------|
| Graph | Medium | DFS + Stack |

---

# 📌 Interview Definition

Topological Sort using DFS is based on the idea:

> A node should be added to the answer **only after all its outgoing neighbours have been processed.**

Instead of using **Indegree + Queue**, we use

- DFS
- Stack

---

# 🧠 Intuition

Suppose

```text
0 → 1 → 2
```

DFS

```text
0

↓

1

↓

2
```

Node

```text
2
```

has no outgoing neighbours.

Push

```text
2
```

Return to

```text
1
```

Push

```text
1
```

Return to

```text
0
```

Push

```text
0
```

Stack

```text
Top

0

1

2
```

Pop everything

```text
0 1 2
```

Correct Topological Order.

---

# Why Stack?

DFS finishes the deepest node first.

The stack reverses the finishing order.

This naturally gives

```text
Parent

↓

Child
```

instead of

```text
Child

↓

Parent
```

---

# Key Idea

Never push a node immediately.

Always

```text
DFS all neighbours

↓

Then push current node into stack.
```

This is called **postorder DFS**.

---

# Algorithm

```text
Build Adjacency List

↓

Visited Array

↓

For every unvisited node

↓

DFS

↓

Visit all neighbours

↓

Push current node into stack

↓

Repeat

↓

Pop stack into answer
```

---

# Code

```cpp
class Solution {
public:

    void dfs(int node,
             vector<vector<int>>& adj,
             stack<int>& st,
             vector<int>& visited)
    {
        visited[node] = 1;

        for(int neighbour : adj[node])
        {
            if(!visited[neighbour])
            {
                dfs(neighbour, adj, st, visited);
            }
        }

        st.push(node);
    }

    vector<int> topoSort(int V, vector<vector<int>>& edges)
    {
        vector<vector<int>> adj(V);

        for(auto edge : edges)
        {
            int u = edge[0];
            int v = edge[1];

            adj[u].push_back(v);
        }

        vector<int> visited(V,0);

        stack<int> st;

        for(int i=0;i<V;i++)
        {
            if(!visited[i])
            {
                dfs(i,adj,st,visited);
            }
        }

        vector<int> ans;

        while(!st.empty())
        {
            ans.push_back(st.top());
            st.pop();
        }

        return ans;
    }
};
```

---

# Dry Run

Graph

```text
3 → 0

1 → 0

2 → 0
```

Outer Loop starts from

```text
0
```

DFS

```text
0
```

No neighbours.

Push

```text
0
```

Stack

```text
0
```

---

Next

```text
1
```

Neighbour

```text
0
```

Already visited.

Push

```text
1
```

Stack

```text
Top

1

0
```

---

Next

```text
2
```

Push

```text
2
```

Stack

```text
Top

2

1

0
```

---

Next

```text
3
```

Push

```text
3
```

Stack

```text
Top

3

2

1

0
```

---

Pop Stack

```text
3 2 1 0
```

Valid Topological Order.

---

# DFS vs Kahn's Algorithm

| DFS Topological Sort | Kahn's Algorithm |
|----------------------|------------------|
| Uses DFS | Uses BFS |
| Uses Stack | Uses Queue |
| No Indegree | Uses Indegree |
| Push after recursion | Push when indegree becomes 0 |
| Based on finishing time | Based on dependencies |

---

# Common Mistakes

## ❌ Pushing before DFS

Wrong

```cpp
st.push(node);

dfs(...)
```

Correct

```cpp
dfs(...);

st.push(node);
```

---

## ❌ Forgetting visited[]

Can cause repeated DFS calls.

---

## ❌ Making graph undirected

Wrong

```cpp
adj[v].push_back(u);
```

Topological Sort works only on directed graphs.

---

## ❌ Forgetting to pop stack

The stack stores nodes in reverse topological order.

Always pop into the answer vector.

---

# Complexity

| Complexity | Value |
|------------|-------|
| Time | **O(V + E)** |
| Auxiliary Space | **O(V)** |

### Why?

- Every vertex is visited once.
- Every edge is processed once.
- Stack stores at most **V** nodes.
- DFS recursion stack is at most **O(V)**.

---

# When to Use DFS Topological Sort

Use this version when

- You are comfortable with DFS.
- The graph is a DAG.
- The problem asks for a valid ordering.

---

# Recognition Pattern

Whenever the problem says

```text
Dependency

Prerequisite

Before

After

Build Order

Scheduling
```

Think

```text
Directed Graph

↓

Topological Sort

↓

DFS + Stack

or

Kahn's Algorithm
```

---

# Key Takeaways

- Works only on **Directed Acyclic Graphs (DAGs)**.
- Visit every neighbour first.
- Push the current node **after** recursion.
- The stack stores nodes in reverse finishing order.
- Popping the stack gives a valid topological ordering.
- Time Complexity: **O(V + E)**.
