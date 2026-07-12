# DFS Patterns Cheat Sheet

This file helps identify **which DFS template** to use based on the graph representation.

---

# Pattern 1 — Adjacency List DFS

### Graph Representation

```text
Edges

↓

Adjacency List
```

Example

```text
0 -- 1
|
2 -- 3
```

Template

```cpp
for(int node = 0; node < V; node++)
{
    if(!visited[node])
        dfs(node);
}
```

DFS

```cpp
for(int neighbour : adj[node])
{
    if(!visited[neighbour])
        dfs(neighbour);
}
```

### Used In

- Connected Components
- Cycle Detection
- Bipartite Graph
- Topological Sort (DFS)
- Many graph traversal problems

---

# Pattern 2 — Grid DFS

### Graph Representation

```text
Grid

↓

Each Cell = One Node
```

Example

```text
1 1 0

1 0 1

0 1 1
```

Template

```cpp
for(row)
{
    for(col)
    {
        if(valid)
            dfs(row,col);
    }
}
```

DFS

```cpp
Up

Down

Left

Right
```

### Used In

- Number of Islands
- Flood Fill

---

# Pattern 3 — Boundary DFS

### Graph Representation

```text
Grid
```

Start DFS **only from boundary cells**.

Boundary

```text
Top Row

Bottom Row

Left Column

Right Column
```

Template

```text
Boundary Traversal

↓

DFS

↓

Final Double Loop
```

Final Loop

```text
Convert / Count / Restore
```

### Used In

- Surrounded Regions
- Number of Enclaves

---

# When Do We Need the Outer Loop?

## Yes

When the graph may be **disconnected**.

```cpp
for(node)
{
    if(!visited[node])
        dfs(node);
}
```

Examples

- Connected Components
- Provinces

---

## No

When the starting node is already given.

Examples

```text
Flood Fill

↓

(sr, sc)
```

```text
Path Exists

↓

Source Node
```

```text
Dijkstra

↓

Source Node
```

---

# Memory Trick

```text
Adjacency List

↓

Outer Loop

↓

DFS

↓

Neighbour Loop
```

```text
Grid

↓

Double Loop

↓

DFS

↓

4 Directions
```

```text
Boundary DFS

↓

Boundary Traversal

↓

DFS

↓

Final Double Loop
```

---

# Quick Recognition Table

| Input Type | Start DFS From | DFS Neighbours |
|------------|----------------|----------------|
| Edge List / Adjacency List | Every Unvisited Node | `adj[node]` |
| Grid | Every Valid Cell | Up, Down, Left, Right |
| Grid (Boundary Problem) | Boundary Cells Only | Up, Down, Left, Right |
| Given Source Node | Only Source | Depends on Graph Type |

---

# Key Takeaways

- **Adjacency List Graph** → Outer loop + DFS + neighbour loop.
- **Grid Graph** → Double loop + DFS + 4 directions.
- **Boundary DFS** → Boundary traversal + DFS + final double loop.
- Always identify **graph representation** first, then decide the DFS template.
