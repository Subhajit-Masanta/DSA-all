# Detect Cycle in Directed Graph (Kahn's Algorithm)

| Topic | Difficulty | Pattern |
|--------|------------|---------|
| Graph | Medium | Topological Sort + Cycle Detection |

---

# 📌 Problem

Given a **Directed Graph**, determine whether it contains a cycle.

---

# 🧠 Intuition

A Directed Graph has **no cycle** if we can perform a complete **Topological Sort**.

Kahn's Algorithm processes nodes with

```text
Indegree = 0
```

If every node gets processed,

```text
No Cycle
```

If some nodes are never processed,

```text
Cycle Exists
```

because those nodes keep waiting for each other.

---

# Key Observation

Think of

```text
indegree[node]
```

as

```text
Remaining Prerequisites
```

If a cycle exists,

those prerequisites never become zero.

---

# Algorithm

```text
Build Adjacency List

↓

Compute Indegree

↓

Push every node having indegree = 0

↓

processed = 0

↓

While Queue not Empty

    Pop node

    processed++

    Visit neighbours

    indegree[neighbour]--

    If indegree becomes 0

        Push neighbour

↓

After BFS

↓

processed == V ?

↓

Yes → No Cycle

No → Cycle Exists
```

---

# Example (No Cycle)

```text
0 → 1 → 2 → 3
```

Indegree

```text
0 : 0
1 : 1
2 : 1
3 : 1
```

Queue

```text
0
```

Process

```text
0 → 1 → 2 → 3
```

Processed

```text
4
```

Total Nodes

```text
4
```

Answer

```text
No Cycle
```

---

# Example (Cycle)

```text
0 → 1

1 → 2

2 → 1
```

Indegree

```text
0 : 0
1 : 2
2 : 1
```

Queue

```text
0
```

Process

```text
0
```

Queue becomes

```text
Empty
```

Nodes

```text
1

2
```

still have

```text
indegree > 0
```

Processed

```text
1
```

Total Nodes

```text
3
```

Since

```text
processed != V
```

Answer

```text
Cycle Exists
```

---

# Why No Infinite Loop?

Nodes are pushed into the queue **only once**, when

```cpp
indegree[node] == 0
```

In a cycle,

```text
1 → 2 → 1
```

Neither node's indegree ever becomes

```text
0
```

So they never enter the queue.

Eventually,

```text
Queue becomes Empty
```

and the BFS stops naturally.

---

# Code Pattern

```cpp
queue<int> q;

for(each node)
{
    if(indegree[node] == 0)
        q.push(node);
}

int processed = 0;

while(!q.empty())
{
    int node = q.front();
    q.pop();

    processed++;

    for(int neigh : adj[node])
    {
        indegree[neigh]--;

        if(indegree[neigh] == 0)
            q.push(neigh);
    }
}

return processed != V;   // Cycle exists
```

---

# Complexity

| Complexity | Value |
|------------|-------|
| Time | **O(V + E)** |
| Space | **O(V + E)** |

---

# Relationship with Other Problems

| Problem | Same Idea |
|---------|-----------|
| Topological Sort | Return ordering |
| Course Schedule (207) | `processed == V` → `true` |
| Course Schedule II (210) | Return ordering if `processed == V` |
| Detect Cycle (Directed) | `processed != V` → Cycle |

---

# Recognition Pattern ⭐⭐⭐⭐⭐

Whenever you see

```text
Directed Graph

+

Cycle Detection
```

Think of two approaches:

1. **DFS + visited[] + pathVisited[]**
2. **Kahn's Algorithm (Topological Sort)**

---

# Key Takeaways

- Uses **Kahn's Algorithm**.
- Queue stores nodes with **indegree = 0**.
- Every processed node removes its outgoing edges.
- Cyclic nodes never reach **indegree = 0**.
- If `processed == V` → No Cycle.
- If `processed < V` → Cycle Exists.
