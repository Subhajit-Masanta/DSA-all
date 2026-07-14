# Detect Cycle in an Undirected Graph (BFS)

| Topic | Difficulty | Pattern |
|--------|------------|---------|
| Graph | Medium | BFS + Parent Tracking |

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

# Example 1

Input

```cpp
V = 4

edges = [[0,1],[0,2],[1,2],[2,3]]
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

Input

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

In an undirected graph,

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

Suppose BFS visits

```text
0

↓

1
```

Now while processing node

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

So we store

```text
(Current Node, Parent)
```

inside the queue.

---

# Queue

Instead of

```cpp
queue<int>
```

use

```cpp
queue<pair<int,int>>
```

Store

```text
(Current Node, Parent)
```

Example

```text
(0,-1)

↓

(1,0)

↓

(2,1)
```

---

# Key Observation

For every neighbour

There are only three cases.

---

## Case 1

Neighbour not visited

↓

Mark visited

↓

Push

```text
(Neighbour, Current Node)
```

---

## Case 2

Neighbour visited

AND

Neighbour is Parent

↓

Ignore

---

## Case 3

Neighbour visited

AND

Neighbour is NOT Parent

↓

Cycle Found

---

# Algorithm

```text
Build Adjacency List

↓

Visited Array

↓

For every Component

↓

Push(Start,-1)

↓

Mark Start Visited

↓

While Queue Not Empty

↓

Pop(Node,Parent)

↓

Visit Every Neighbour

↓

Neighbour Not Visited

↓

Visited = true

↓

Push(Neighbour,Node)

↓

Neighbour Visited

↓

Neighbour != Parent

↓

Cycle Found

↓

Return true

↓

Finished All Components

↓

Return false
```

---

# Code

```cpp
class Solution {
public:

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

        queue<pair<int,int>> q;

        for(int i=0;i<V;i++)
        {
            if(!visited[i])
            {
                q.push({i,-1});
                visited[i]=1;

                while(!q.empty())
                {
                    auto curr=q.front();
                    q.pop();

                    int node=curr.first;
                    int parent=curr.second;

                    for(int neighbour:adj[node])
                    {
                        if(!visited[neighbour])
                        {
                            visited[neighbour]=1;
                            q.push({neighbour,node});
                        }
                        else if(neighbour!=parent)
                        {
                            return true;
                        }
                    }
                }
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
```

Queue

```text
(0,-1)
```

Visited

```text
0
```

---

Pop

```text
(0,-1)
```

Neighbours

```text
1

2
```

Push

```text
(1,0)

(2,0)
```

Visited

```text
0

1

2
```

---

Pop

```text
(1,0)
```

Neighbours

```text
0
```

Parent

↓

Ignore

Neighbour

```text
2
```

Already visited

AND

```text
2 != Parent
```

Cycle Found.

Return

```text
true
```

---

# DFS vs BFS

| DFS | BFS |
|------|-----|
| Recursion | Queue |
| Pass Parent in Function | Store Parent in Queue |
| DFS(Node, Parent) | Queue<(Node, Parent)> |
| O(V+E) | O(V+E) |

---

# Common Mistakes

## ❌ Using queue<int>

Need

```cpp
queue<pair<int,int>>
```

to remember the parent.

---

## ❌ Marking visited after pop

Wrong

```cpp
Pop

↓

Visited
```

Correct

```cpp
Visited

↓

Push
```

This prevents duplicate insertions.

---

## ❌ Forgetting Parent

Every graph appears to have a cycle.

---

## ❌ Returning after first component

Always check every connected component.

---

# Complexity

| Complexity | Value |
|------------|-------|
| Time | **O(V + E)** |
| Auxiliary Space | **O(V)** |

### Why?

- Every vertex is visited once.
- Every edge is processed once.
- Queue stores at most **V** nodes.
- Visited array uses **O(V)**.

---

# Key Takeaways

- BFS stores **(Node, Parent)**.
- Ignore the edge back to the parent.
- If a visited neighbour is **not** the parent, a cycle exists.
- Mark a node visited **before** pushing it into the queue.
- Always process all connected components.
