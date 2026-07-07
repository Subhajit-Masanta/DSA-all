# Graph DFS Traversal

## Problem

Given an undirected graph, traverse and print all nodes using Depth First Search (DFS).

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

Use DFS when you need to:

- Explore a complete connected region/component.
- Go deep along one path before returning.
- Check reachability.
- Count connected components.
- Explore graph paths.

---

## Core Idea

For the current node:

```text
1. MARK it visited.
2. PRINT/process it.
3. LOOP through its neighbours.
4. RECURSE for every unvisited neighbour.
```

Memory trick:

```text
MARK → PRINT → LOOP → RECURSE
```

---

## Data Structure Used

- `adj[u]` → all direct neighbours of node `u`.
- `visited[u]` → `1` if node `u` has already been visited.
- Recursion call stack → stores the current DFS path.

---

## Algorithm

1. Build the adjacency list.
2. Create `visited` initialized with `0`.
3. Call DFS from the starting node.
4. Mark the current node visited.
5. Print the current node.
6. Visit every unvisited neighbour recursively.

---

## Code

```cpp
#include <iostream>
#include <vector>
using namespace std;

void dfs(int node, vector<vector<int>>& adj, vector<int>& visited)
{
    // Mark current node visited
    visited[node] = 1;

    // Print/process current node
    cout << node << " ";

    // Check all neighbours
    for(int neighbour : adj[node])
    {
        if(visited[neighbour] == 0)
        {
            dfs(neighbour, adj, visited);
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

    // Start DFS from node 0
    dfs(0, adj, visited);

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
dfs(0)
visited = [1,0,0,0,0]
print 0
```

Go to neighbour `1`:

```text
dfs(1)
visited = [1,1,0,0,0]
print 1
```

Neighbour `0` is already visited, so skip it.

Go to `3`:

```text
dfs(3)
visited = [1,1,0,1,0]
print 3
```

Return to `1`, then return to `0`.

Now visit `2`, then `4`.

Output:

```text
0 1 3 2 4
```

---

## Disconnected Graph

Calling only:

```cpp
dfs(0, adj, visited);
```

visits only the component containing node `0`.

To traverse the entire graph:

```cpp
for(int node = 0; node < n; node++)
{
    if(visited[node] == 0)
    {
        dfs(node, adj, visited);
    }
}
```

Every new DFS call from the outer loop starts a new connected component.

---

## Common Mistakes

- Forgetting `visited` causes infinite recursion in cyclic graphs.
- Forgetting both edge insertions for an undirected graph.
- Calling DFS without checking whether the neighbour is already visited.
- Using only `dfs(0)` when the graph may be disconnected.

---

## Complexity

```text
Time  : O(V + E)
Space : O(V)
```

---

## Key Takeaways

```text
Tree DFS:
left → right

Graph DFS:
loop through all neighbours
```

Core template:

```cpp
visited[node] = 1;

for(int neighbour : adj[node])
{
    if(!visited[neighbour])
        dfs(neighbour, adj, visited);
}
```
