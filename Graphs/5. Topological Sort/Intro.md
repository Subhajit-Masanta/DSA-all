# Topological Sort — Introduction

---

# 📌 Interview Definition

Topological Sort is a **linear ordering of vertices** in a **Directed Acyclic Graph (DAG)** such that for every directed edge

```text
u → v
```

vertex

```text
u
```

always appears **before**

```text
v
```

---

# Works Only On

✅ Directed Graph

✅ Acyclic Graph (DAG)

❌ Not possible if a cycle exists.

---

# Think of Dependencies

Instead of thinking about graphs, think about tasks.

Example

```text
Wake Up

↓

Brush

↓

Breakfast

↓

College
```

Valid Order

```text
Wake Up

↓

Brush

↓

Breakfast

↓

College
```

Invalid Order

```text
College

↓

Brush
```

Reason

```text
Brush depends on Wake Up.
```

Topological Sort simply finds one valid order satisfying all dependencies.

---

# Example

Graph

```text
5 → 2 → 3
↓
0
```

Valid Orders

```text
5 2 3 0
```

or

```text
5 0 2 3
```

Both are correct.

There can be **multiple valid answers**.

---

# Important Properties

- Works only for **Directed Graphs**.
- Graph must be a **DAG**.
- Multiple valid topological orders may exist.
- If a cycle exists, topological sorting is impossible.

---

# Common Applications

Whenever you hear

- Course Prerequisites
- Task Scheduling
- Job Scheduling
- Build Order
- Package Installation
- Compilation Order
- Project Dependencies

Immediately think

```text
Topological Sort
```

---

# Input Representation

Given

```cpp
edges = {
    {4,1},
    {4,0},
    {5,0},
    {5,2}
};
```

Means

```text
4 → 1

4 → 0

5 → 0

5 → 2
```

Build adjacency list

```cpp
adj[u].push_back(v);
```

**Do NOT add**

```cpp
adj[v].push_back(u);
```

because the graph is **directed**.

---

# Algorithms to Find Topological Sort

### 1. Kahn's Algorithm (BFS)

Uses

- Indegree
- Queue

Time

```text
O(V + E)
```

---

### 2. DFS Topological Sort

Uses

- DFS
- Stack

Time

```text
O(V + E)
```

---

# Recognition Pattern ⭐⭐⭐⭐⭐

Whenever the problem contains words like

```text
Before

After

Dependency

Prerequisite

Order

Schedule

Build Order
```

Immediately think

```text
Directed Graph

+

Topological Sort
```

---

# Key Takeaways

- Topological Sort gives a valid execution order.
- Every edge `u → v` means **u must come before v**.
- Only possible in a **Directed Acyclic Graph (DAG)**.
- There may be multiple correct orders.
- Two standard methods:
  - Kahn's Algorithm (BFS)
  - DFS + Stack
