# Shortest Path in Unweighted Graph (BFS)

| Topic | Difficulty | Pattern |
|--------|------------|---------|
| Graph | Medium | BFS + Distance Array |

---

# Problem

Given an **undirected graph** where every edge has **unit weight (1)**, find the length of the shortest path from **src** to **dest**.

If no path exists, return

```text
-1
```

---

# Key Observation

Every edge has

```text
Weight = 1
```

Since every edge costs the same,

```text
Shortest Cost

=

Fewest Edges
```

---

# Why BFS?

BFS explores nodes

```text
Level by Level
```

Example

```text
        0
      /   \
     1     3
     |     |
     2     4
      \
       6
        \
         8
```

Levels

```text
Level 0 : 0

Level 1 : 1 3

Level 2 : 2 4

Level 3 : 6

Level 4 : 8
```

The first time BFS reaches a node,

it has already found the path with the minimum number of edges.

---

# Why Not DFS?

DFS may travel deep into one branch first.

Example

```text
0

|

1

|

2

|

3

|

4

|

5

 \

 6
```

DFS might reach

```text
6
```

after many edges,

even though another path could be much shorter.

BFS always guarantees the shortest path when every edge has equal weight.

---

# Algorithm

```text
Build Adjacency List

↓

Create Distance Array

↓

Initialize

dist[src] = 0

↓

Push src into Queue

↓

While Queue is not Empty

↓

Visit Neighbours

↓

If Neighbour is Unvisited

↓

dist[neighbour] = dist[node] + 1

↓

Push into Queue

↓

Return dist[dest]
```

---

# Dry Run

Graph

```text
0 --- 1 --- 2 --- 6 --- 8
 \

 3
```

Source

```text
0
```

Initial

```text
dist

0 -1 -1 -1 -1 -1 -1 -1 -1
```

Queue

```text
0
```

Process

```text
0

↓

1 3
```

Distances

```text
0 1 -1 1 -1 -1 -1 -1 -1
```

Process

```text
1

↓

2
```

Distances

```text
0 1 2 1 -1 -1 -1 -1 -1
```

Process

```text
2

↓

6
```

Distances

```text
0 1 2 1 -1 -1 3 -1 -1
```

Process

```text
6

↓

8
```

Distances

```text
0 1 2 1 -1 -1 3 -1 4
```

Answer

```text
4
```

---

# Important Formula

Whenever a neighbour is visited for the first time

```cpp
dist[neighbour] = dist[node] + 1;
```

Reason

```text
Current Distance

+

One More Edge
```

---

# Implementation Pattern

```cpp
vector<vector<int>> adj(V);

vector<int> dist(V,-1);

queue<int> q;

dist[src]=0;

q.push(src);

while(!q.empty())
{
    int node=q.front();
    q.pop();

    for(int neigh:adj[node])
    {
        if(dist[neigh]==-1)
        {
            dist[neigh]=dist[node]+1;
            q.push(neigh);
        }
    }
}
```

---

# Why Distance Array Instead of Visited?

A node is

```text
Visited

⇔

Distance != -1
```

So

```cpp
visited[]
```

is unnecessary.

Instead

```cpp
if(dist[neigh]==-1)
```

means

```text
Not Visited Yet
```

This saves one extra array.

---

# Complexity

| Complexity | Value |
|------------|-------|
| Time | **O(V + E)** |
| Space | **O(V + E)** |

---

# Recognition Pattern ⭐⭐⭐⭐⭐

Whenever you see

```text
Graph

+

All Edge Weights = 1

+

Shortest Path
```

Immediately think

```text
BFS
```

NOT

```text
Dijkstra
```

---

# Difference from Normal BFS

Normal BFS

```text
Visit Every Node
```

Shortest Path BFS

```text
Maintain Distance Array

↓

Update

dist[neighbour] = dist[node] + 1
```

---

# Common Mistakes

## ❌ Using DFS

DFS does not guarantee the shortest path.

---

## ❌ Using Dijkstra

Works,

but is unnecessary because every edge has the same weight.

BFS is simpler and faster.

---

## ❌ Forgetting to initialize

```cpp
dist[src]=0;
```

---

## ❌ Updating distance multiple times

Only update when the node is visited for the first time.

```cpp
if(dist[neigh]==-1)
```

---

# Key Takeaways

- Unit weight graph ⇒ BFS.
- First visit in BFS is always the shortest.
- Store shortest distance in `dist[]`.
- Transition:

```cpp
dist[neigh] = dist[node] + 1;
```

- Return `dist[dest]`; if it remains `-1`, no path exists.
