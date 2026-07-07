# Graph BFS Traversal

## Problem

Given an undirected graph, traverse and print all nodes using Breadth First Search (BFS).

Example input:

```text
5 4
0 1
0 2
1 3
2 4
```

Graph:

```text
    3
    |
    1
    |
    0
    |
    2
    |
    4
```

Adjacency list:

```text
adj[0] = [1, 2]
adj[1] = [0, 3]
adj[2] = [0, 4]
adj[3] = [1]
adj[4] = [2]
```

---

## Recognition

Use BFS when you need:

- Level-by-level traversal.
- Minimum number of edges/moves in an unweighted graph.
- Nearest or shortest unweighted path.
- Multi-source traversal.

---

## Core Idea

For the starting node:

```text
1. MARK visited.
2. PUSH into queue.
3. POP a node.
4. PRINT/process it.
5. PUSH all unvisited neighbours.
```

Memory trick:

```text
MARK → PUSH → POP → LOOP → PUSH
```

---

## Data Structure Used

- `adj[u]` → all direct neighbours of node `u`.
- `visited[u]` → whether node `u` has already been discovered.
- `q` → nodes discovered but still waiting to be processed.

Important:

```text
queue front = next node to process
```

---

## Algorithm

1. Build the adjacency list.
2. Create `visited` initialized with `0`.
3. Mark the starting node visited.
4. Push it into the queue.
5. While the queue is not empty:
   - Take the front node.
   - Remove it.
   - Print/process it.
   - Push every unvisited neighbour.

---

## Code

```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

void bfs(int start, vector<vector<int>>& adj, vector<int>& visited)
{
    queue<int> q;

    // Mark when pushing
    visited[start] = 1;
    q.push(start);

    while(!q.empty())
    {
        int node = q.front();
        q.pop();

        cout << node << " ";

        for(int neighbour : adj[node])
        {
            if(visited[neighbour] == 0)
            {
                visited[neighbour] = 1;
                q.push(neighbour);
            }
        }
    }
}

int main()
{
    int n, m;
    cin >> n >> m;

    vector<vector<int>> adj(n);

    // Build undirected graph
    for(int i = 0; i < m; i++)
    {
        int u, v;
        cin >> u >> v;

        adj[u].push_back(v);
        adj[v].push_back(u);
    }

    vector<int> visited(n, 0);

    // Start BFS from node 0
    bfs(0, adj, visited);

    return 0;
}
```

---

## One Dry Run

Input:

```text
5 4
0 1
0 2
1 3
2 4
```

Start:

```text
visited = [1,0,0,0,0]
queue   = [0]
```

Pop `0` and push its unvisited neighbours:

```text
print   = 0
visited = [1,1,1,0,0]
queue   = [1,2]
```

Pop `1`:

```text
print   = 0 1
visited = [1,1,1,1,0]
queue   = [2,3]
```

Pop `2`:

```text
print   = 0 1 2
visited = [1,1,1,1,1]
queue   = [3,4]
```

Then pop `3` and `4`.

Output:

```text
0 1 2 3 4
```

---

## Why Mark Visited When Pushing?

Correct:

```cpp
visited[neighbour] = 1;
q.push(neighbour);
```

If we wait until popping, the same node may be pushed into the queue multiple times by different neighbours.

Rule:

```text
The moment a node enters the queue, mark it visited.
```

---

## Disconnected Graph

To traverse the entire graph:

```cpp
for(int node = 0; node < n; node++)
{
    if(visited[node] == 0)
    {
        bfs(node, adj, visited);
    }
}
```

---

## Common Mistakes

- Marking visited when popping instead of pushing.
- Forgetting `q.pop()`.
- Forgetting both edge insertions for an undirected graph.
- Using only `bfs(0)` for a disconnected graph.

---

## Complexity

```text
Time  : O(V + E)
Space : O(V)
```

---

## Key Takeaways

```text
DFS → recursion / stack → goes deep

BFS → queue → goes level by level
```

Core BFS template:

```cpp
visited[start] = 1;
q.push(start);

while(!q.empty())
{
    int node = q.front();
    q.pop();

    for(int neighbour : adj[node])
    {
        if(!visited[neighbour])
        {
            visited[neighbour] = 1;
            q.push(neighbour);
        }
    }
}
```
