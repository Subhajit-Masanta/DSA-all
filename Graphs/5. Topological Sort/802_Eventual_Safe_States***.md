# 802. Eventual Safe States

| Topic | Difficulty | Pattern |
|--------|------------|---------|
| Graph | Medium | Reverse Graph + Kahn's Algorithm |

---

# 📌 Problem

A node is called **Safe** if **every possible path** starting from that node eventually reaches a **Terminal Node**.

A **Terminal Node** is a node with **no outgoing edges**.

Return all safe nodes in **ascending order**.

---

# 🧠 Key Observation

We already know one thing:

```text
Terminal Nodes are always Safe.
```

Now we need to find

```text
Who can reach only Safe Nodes?
```

The safety information moves

```text
Terminal

↓

Previous Nodes
```

This is opposite to the original edge direction.

Therefore,

```text
Reverse the Graph.
```

---

# Why Reverse the Graph?

Original

```text
4 → 5
```

Suppose

```text
5
```

is already safe.

Can we reach

```text
4
```

using the original graph?

No.

Original graph only allows

```text
4 → 5
```

After reversing

```text
5 → 4
```

Now starting from

```text
5
```

we can discover

```text
4
```

---

# Main Idea

Original graph starts from

```text
Outdegree = 0
```

(Known Safe Nodes)

But Kahn's Algorithm starts from

```text
Indegree = 0
```

So,

```text
Reverse Graph

↓

Original Outdegree

=

Reversed Indegree
```

Now we can use the same Topological Sort.

---

# Algorithm

```text
Reverse every edge

↓

For every original edge

u → v

Store

v → u

↓

Original Outdegree

becomes

Reversed Indegree

↓

Push every node whose indegree = 0
(Terminal Nodes)

↓

Run Kahn's Algorithm

↓

Whenever a node's indegree becomes 0

↓

It is also Safe

↓

Store Answer

↓

Sort Answer

↓

Return
```

---

# Reverse Graph Construction

Original

```text
u → v
```

Reverse

```cpp
revGraph[v].push_back(u);
```

At the same time

```cpp
indegree[u]++;
```

because

```text
Original Outdegree

↓

Reversed Indegree
```

---

# Code

```cpp
class Solution {
public:
    vector<int> eventualSafeNodes(vector<vector<int>>& graph) {

        int V = graph.size();

        vector<vector<int>> revGraph(V);
        vector<int> indegree(V,0);

        for(int u=0;u<V;u++)
        {
            for(int v:graph[u])
            {
                revGraph[v].push_back(u);

                indegree[u]++;
            }
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

            for(int neighbour:revGraph[node])
            {
                indegree[neighbour]--;

                if(indegree[neighbour]==0)
                    q.push(neighbour);
            }
        }

        sort(ans.begin(),ans.end());

        return ans;
    }
};
```

---

# Dry Run

Input

```text
0 → 1

1 → 3

3 → 4

4 → 1

1 → 2
```

Terminal Node

```text
2
```

Reverse Graph

```text
1 ← 0

3 ← 1

4 ← 3

1 ← 4

2 ← 1
```

becomes

```text
1 → 0

3 → 1

4 → 3

1 → 4

2 → 1
```

Initial

```text
Queue

2
```

Process

```text
2
```

Decrease indegree of

```text
1
```

Still not

```text
0
```

because

```text
1 → 3
```

still exists.

Queue becomes empty.

Safe Nodes

```text
2
```

Correct.

---

# Example

Input

```text
0 → 1

1 → 2

2
```

Reverse

```text
2 → 1 → 0
```

Queue

```text
2
```

Process

```text
2

↓

1

↓

0
```

Safe Nodes

```text
0 1 2
```

---

# Common Mistakes

## ❌ Forgetting to Reverse

Original graph cannot move

```text
Safe Node

↓

Parent
```

---

## ❌ Using Original Indegree

Need

```text
Original Outdegree

↓

Reversed Indegree
```

So

```cpp
indegree[u]++;
```

not

```cpp
indegree[v]++;
```

---

## ❌ Creating Answer Vector Like

```cpp
vector<int> ans(V);
```

This creates

```text
0 0 0 ...
```

Always use

```cpp
vector<int> ans;
```

---

# Complexity

| Complexity | Value |
|------------|-------|
| Time | **O(V + E)** |
| Space | **O(V + E)** |

---

# Comparison

| Course Schedule | Eventual Safe States |
|-----------------|----------------------|
| Start from indegree = 0 | Start from terminal nodes (outdegree = 0) |
| Original Graph | Reverse Graph |
| Return valid ordering | Return safe nodes |
| Dependency flows forward | Safety flows backward |

---

# Recognition Pattern ⭐⭐⭐⭐⭐

Whenever a problem says

```text
Known Good Nodes

↓

Find all nodes that eventually reach them
```

Think

```text
Reverse Graph

↓

Kahn's Algorithm
```

---

# Key Takeaways

- Terminal nodes are always safe.
- Safety information flows **backwards**.
- Reverse every edge.
- Original outdegree becomes reversed indegree.
- Run Kahn's Algorithm on the reversed graph.
- Every processed node is a safe node.
- Sort the answer before returning.
