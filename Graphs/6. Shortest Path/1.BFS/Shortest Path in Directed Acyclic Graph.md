# Shortest Path in Directed Acyclic Graph (GFG)

| Topic | Difficulty | Pattern |
|--------|------------|---------|
| Graph | Medium | Topological Sort + Edge Relaxation |

---

# Problem

Given a **Directed Acyclic Graph (DAG)** with weighted edges, find the shortest distance from

```text
Source = 0
```

to every vertex.

If a vertex cannot be reached,

return

```text
-1
```

for that vertex.

---

# Key Observation

Unlike the previous problem,

the graph is

```text
Weighted
```

Example

```text
0 --100--> 1

 \

  \--1--> 2 --1--> 1
```

The path with

```text
Fewer Edges
```

is

```text
0 → 1
```

Cost

```text
100
```

But the shortest path is

```text
0 → 2 → 1
```

Cost

```text
2
```

Therefore

```text
BFS does NOT work.
```

---

# Why Not Dijkstra?

We can use Dijkstra.

But this graph has a special property.

```text
Directed

+

Acyclic
```

Since there are **no cycles**, we can process vertices in

```text
Topological Order.
```

---

# Main Idea

Suppose Topological Order is

```text
0

↓

4

↓

1

↓

2

↓

5

↓

3
```

When processing

```text
Node = 2
```

all nodes that can reach

```text
2
```

have already been processed.

Therefore,

its shortest distance is already known.

Now we only need to update its neighbours.

---

# Edge Relaxation

Suppose

```text
dist[node]
```

is already known.

There is an edge

```text
node --wt--> neighbour
```

Current distance

```text
dist[neighbour]
```

New possible distance

```text
dist[node] + wt
```

If the new path is shorter,

update it.

```cpp
if(dist[node] != INT_MAX &&
   dist[neighbour] > dist[node] + wt)
{
    dist[neighbour] = dist[node] + wt;
}
```

This process is called

```text
Edge Relaxation
```

---

# Algorithm

```text
Build Weighted Graph

↓

Compute Indegree

↓

Topological Sort (Kahn)

↓

Distance[source] = 0

↓

Process nodes in Topological Order

↓

Relax all outgoing edges

↓

Convert INT_MAX to -1

↓

Return Distance Array
```

---

# Code

```cpp
class Solution {
public:
    vector<int> shortestPath(int V, int E, vector<vector<int>>& edges) {

        vector<vector<pair<int,int>>> adj(V);
        vector<int> indegree(V,0);

        for(auto edge : edges)
        {
            int u = edge[0];
            int v = edge[1];
            int wt = edge[2];

            adj[u].push_back({v,wt});
            indegree[v]++;
        }

        queue<int> q;

        for(int i=0;i<V;i++)
        {
            if(indegree[i]==0)
                q.push(i);
        }

        vector<int> dist(V,INT_MAX);

        dist[0]=0;

        while(!q.empty())
        {
            int node=q.front();
            q.pop();

            for(auto edge:adj[node])
            {
                int neigh=edge.first;
                int wt=edge.second;

                if(dist[node]!=INT_MAX &&
                   dist[neigh] > dist[node]+wt)
                {
                    dist[neigh]=dist[node]+wt;
                }

                indegree[neigh]--;

                if(indegree[neigh]==0)
                    q.push(neigh);
            }
        }

        for(int i=0;i<V;i++)
        {
            if(dist[i]==INT_MAX)
                dist[i]=-1;
        }

        return dist;
    }
};
```

---

# Dry Run

Graph

```text
0

|2

↓

1

|3

↓

2

↑

2

|

4

↑

1

|

0
```

Edges

```text
0 → 1 (2)

0 → 4 (1)

4 → 2 (2)

1 → 2 (3)
```

Topological Order

```text
0

↓

4

↓

1

↓

2
```

Initially

```text
dist

0 INF INF INF INF
```

---

Process

```text
0
```

Update

```text
1 = 2

4 = 1
```

---

Process

```text
4
```

Update

```text
2 = 3
```

---

Process

```text
1
```

Possible distance

```text
2 + 3 = 5
```

Already

```text
3
```

No update.

Final

```text
0 2 3 -1 1
```

---

# Why Topological Sort?

Because a DAG has

```text
No Cycles.
```

When a node appears in Topological Order,

all its parent vertices have already been processed.

So its shortest distance is finalized before processing its outgoing edges.

---

# Why Relaxation?

Suppose

```text
Current Distance = 10
```

New path

```text
7
```

Always choose the smaller one.

```cpp
dist[neigh] = min(dist[neigh],
                  dist[node]+wt);
```

This idea is called

```text
Edge Relaxation.
```

It is the foundation of

- Dijkstra
- Bellman-Ford
- Floyd-Warshall

---

# Common Mistakes

## ❌ Using BFS

BFS works only when every edge has

```text
Weight = 1
```

---

## ❌ Forgetting Topological Order

Relaxation is correct only when vertices are processed in topological order.

---

## ❌ Overflow

Wrong

```cpp
dist[node]+wt
```

when

```cpp
dist[node]==INT_MAX
```

Always check

```cpp
dist[node]!=INT_MAX
```

before relaxing.

---

## ❌ Returning INT_MAX

Convert unreachable vertices to

```text
-1
```

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
Directed Graph

+

Acyclic Graph (DAG)

+

Weighted Edges

+

Shortest Path
```

Immediately think

```text
Topological Sort

↓

Edge Relaxation
```

Do **NOT** use BFS.

Dijkstra also works, but Topological Sort is simpler and runs in **O(V + E)**.

---

# Key Takeaways

- BFS works only for unit-weight graphs.
- A DAG allows vertices to be processed in topological order.
- Process each node once in topological order.
- Relax every outgoing edge.
- Always check `dist[node] != INT_MAX` before relaxing.
- Edge Relaxation is the core idea behind all shortest-path algorithms.
