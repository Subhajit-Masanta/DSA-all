# Dijkstra Algorithm (Priority Queue)

| Topic | Difficulty | Pattern |
|--------|------------|---------|
| Graph | Medium | Dijkstra + Min Heap |

---

# Problem

Given a **weighted graph** with **non-negative edge weights**, find the shortest distance from a source node to every other node.

---

# Key Observation

Edge weights are

```text
Positive / Non-Negative
```

Different paths can have different total costs.

So,

```text
Fewest Edges

≠

Minimum Cost
```

Hence,

```text
BFS ❌
```

Instead use

```text
Dijkstra ✅
```

---

# Why Dijkstra?

At every step,

```text
Always process the node having

Minimum Current Distance
```

Once a node with the minimum distance is processed,

its shortest distance is finalized.

---

# Why Not BFS?

Example

```text
      5
0 ----------> 1

 \

  1

   \

    v

     2 -------> 1
         1
```

BFS chooses

```text
0 → 1

Cost = 5
```

But the shortest path is

```text
0 → 2 → 1

Cost = 2
```

BFS minimizes

```text
Edges
```

Dijkstra minimizes

```text
Total Weight
```

---

# Why Priority Queue?

We always need the node having

```text
Minimum Distance
```

Priority Queue (Min Heap) gives it in

```text
O(log V)
```

instead of

```text
O(V)
```

---

# Priority Queue Structure

We store

```text
(distance, node)
```

Declaration

```cpp
priority_queue<
pair<int,int>,
vector<pair<int,int>>,
greater<pair<int,int>>
> pq;
```

Example

```text
(0,0)

(7,2)

(2,1)

(5,3)
```

Top

```text
(0,0)
```

After Pop

```text
(2,1)
```

---

# Algorithm

```text
Build Adjacency List

↓

Create Distance Array

↓

dist[src] = 0

↓

Push

(distance,node)

↓

While PQ not Empty

↓

Pop Minimum Distance Node

↓

Ignore stale entry

↓

Relax every neighbour

↓

If shorter path found

↓

Update distance

↓

Push updated (distance,node)

↓

Return dist[]
```

---

# Important Formula (Relaxation)

Whenever a better path is found

```cpp
if(dist[neigh] > currDist + wt)
{
    dist[neigh] = currDist + wt;
    pq.push({dist[neigh], neigh});
}
```

Reason

```text
Current Shortest Distance

+

Edge Weight
```

---

# Dry Run

Graph

```text
          10
0 ----------------> 1
 \                 ^
  \                |
   1               |2
    \              |
     v             |
      2------------
```

Source

```text
0
```

---

### Initial

Distance

```text
[0, INF, INF]
```

Priority Queue

```text
(0,0)
```

---

### Pop (0,0)

Neighbours

```text
1 via 10

2 via 1
```

Update

```text
dist =

[0,10,1]
```

Push

```text
(10,1)

(1,2)
```

Priority Queue

```text
(1,2)

(10,1)
```

---

### Pop (1,2)

Neighbour

```text
1

Current = 10

New = 1 + 2 = 3
```

Better path found.

Update

```text
dist =

[0,3,1]
```

Push

```text
(3,1)
```

Priority Queue

```text
(3,1)

(10,1)
```

Notice

```text
Node 1 appears twice.
```

This is completely normal.

---

### Pop (3,1)

Current distance equals

```text
dist[1]
```

So process it.

No better updates.

Priority Queue

```text
(10,1)
```

---

### Pop (10,1)

Current

```text
currDist = 10

dist[1] = 3
```

Since

```cpp
10 > 3
```

this entry is

```text
Stale (Outdated)
```

Skip it.

```cpp
if(currDist > dist[node])
    continue;
```

Queue becomes empty.

Final Answer

```text
[0,3,1]
```

---

# Why Multiple Entries?

Whenever we discover a shorter path,

we simply push

```text
(newDistance,node)
```

without removing the old one.

Example

```text
(10,1)

↓

Found better path

↓

(3,1)
```

Priority Queue

```text
(3,1)

(10,1)
```

Old entries automatically become stale.

---

# Why No visited[] Array?

Instead of marking nodes visited,

we simply ignore outdated entries.

```cpp
if(currDist > dist[node])
    continue;
```

Reason

```text
Priority Queue

↓

May contain duplicate entries

↓

Smallest distance is processed first

↓

Older larger-distance entries become stale

↓

Skip them
```

So,

```cpp
visited[]
```

is unnecessary.

---

# Implementation Pattern

```cpp
vector<vector<pair<int,int>>> adj(V);

vector<int> dist(V, INT_MAX);

dist[src] = 0;

priority_queue<
pair<int,int>,
vector<pair<int,int>>,
greater<pair<int,int>>
> pq;

pq.push({0, src});

while(!pq.empty())
{
    auto [currDist, node] = pq.top();
    pq.pop();

    if(currDist > dist[node])
        continue;

    for(auto [neigh, wt] : adj[node])
    {
        if(dist[neigh] > currDist + wt)
        {
            dist[neigh] = currDist + wt;
            pq.push({dist[neigh], neigh});
        }
    }
}

return dist;
```

---

# Complexity

| Complexity | Value |
|------------|-------|
| Time | **O((V + E) log V)** |
| Space | **O(V + E)** |

---

# Recognition Pattern ⭐⭐⭐⭐⭐

Whenever you see

```text
Weighted Graph

+

Shortest Path

+

All Edge Weights ≥ 0
```

Immediately think

```text
Dijkstra
```

NOT

```text
BFS
```

---

# Common Mistakes

## ❌ Using BFS

BFS minimizes edges,

not total weight.

---

## ❌ Storing

```text
(node,distance)
```

instead of

```text
(distance,node)
```

Priority Queue must sort by distance.

---

## ❌ Forgetting

```cpp
if(currDist > dist[node])
    continue;
```

This causes unnecessary processing of stale entries.

---

## ❌ Not pushing updated distance

After relaxation

```cpp
pq.push({dist[neigh], neigh});
```

must be called.

---

## ❌ Using Dijkstra for Negative Weights

Dijkstra works only when

```text
All Edge Weights ≥ 0
```

Use

```text
Bellman-Ford
```

for graphs containing negative edges.

---

# Key Takeaways

- Weighted graph + shortest path + non-negative weights ⇒ Dijkstra.
- Priority Queue stores

```text
(distance, node)
```

- Always process the smallest current distance.
- Relaxation:

```cpp
dist[neigh] = currDist + wt;
```

- Duplicate entries are normal.
- Skip stale entries using

```cpp
if(currDist > dist[node])
    continue;
```

- No separate `visited[]` array is required.
