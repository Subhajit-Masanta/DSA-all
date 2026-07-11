# Graph Input and Construction

## 1. Basic Graph Input

Most graph questions give:

```text
n m
u1 v1
u2 v2
...
```

Example:

```text
5 4
0 1
0 2
1 3
2 4
```

* `n` → number of nodes
* `m` → number of edges
* `u v` → one edge between nodes `u` and `v`

```cpp
int n, m;
cin >> n >> m;

for(int i = 0; i < m; i++)
{
    int u, v;
    cin >> u >> v;
}
```

After reading `u` and `v`, store them according to the graph type.

---

# 2. The 4 Main Graph Types

## A. Undirected + Unweighted

For an edge:

```text
u ----- v
```

Travel is possible both ways.

### Storage

```cpp
vector<vector<int>> adj(n);
```

### Construction

```cpp
for(int i = 0; i < m; i++)
{
    int u, v;
    cin >> u >> v;

    adj[u].push_back(v);
    adj[v].push_back(u);
}
```

Example edges:

```text
0 1
0 2
```

Final adjacency list:

```text
adj[0] = [1, 2]
adj[1] = [0]
adj[2] = [0]
```

Every undirected edge is stored twice because both directions are possible.

---

## B. Directed + Unweighted

For an edge:

```text
u -----> v
```

Travel is possible only from `u` to `v`.

### Storage

```cpp
vector<vector<int>> adj(n);
```

### Construction

```cpp
for(int i = 0; i < m; i++)
{
    int u, v;
    cin >> u >> v;

    adj[u].push_back(v);
}
```

Example edges:

```text
0 1
0 2
```

Final adjacency list:

```text
adj[0] = [1, 2]
adj[1] = []
adj[2] = []
```

`adj[u]` stores nodes directly reachable from `u`.

---

# 3. Access and Print Unweighted Graph

For both directed and undirected unweighted graphs:

```cpp
for(int i = 0; i < n; i++)
{
    cout << i << " -> ";

    for(int neighbour : adj[i])
    {
        cout << neighbour << " ";
    }

    cout << endl;
}
```

If:

```text
adj[0] = [1, 2]
```

then:

```cpp
for(int neighbour : adj[0])
```

runs as:

```text
Iteration 1: neighbour = 1
Iteration 2: neighbour = 2
```

### Direct Access

```cpp
adj[0][0]   // 1
adj[0][1]   // 2
```

### Key Meaning

```text
adj[u] → all direct neighbours of node u
```

Directed vs undirected changes **edge insertion**, not how the adjacency list is accessed or printed.

---

# 4. Undirected + Weighted

Input edge:

```text
u v weight
```

Example:

```text
0 1 7
```

means:

```text
0 ----- 1
   7
```

### Storage

```cpp
vector<vector<pair<int, int>>> adj(n);
```

Each pair stores:

```text
(neighbour, weight)
```

### Construction

```cpp
for(int i = 0; i < m; i++)
{
    int u, v, wt;
    cin >> u >> v >> wt;

    adj[u].push_back({v, wt});
    adj[v].push_back({u, wt});
}
```

Example:

```text
4 3
0 1 7
0 2 4
2 3 9
```

Final adjacency list:

```text
adj[0] = [(1, 7), (2, 4)]
adj[1] = [(0, 7)]
adj[2] = [(0, 4), (3, 9)]
adj[3] = [(2, 9)]
```

---

# 5. Directed + Weighted

For:

```text
u -----> v
     weight
```

store only one direction.

### Storage

```cpp
vector<vector<pair<int, int>>> adj(n);
```

### Construction

```cpp
for(int i = 0; i < m; i++)
{
    int u, v, wt;
    cin >> u >> v >> wt;

    adj[u].push_back({v, wt});
}
```

---

# 6. Access and Print Weighted Graph

For both directed and undirected weighted graphs:

```cpp
for(int i = 0; i < n; i++)
{
    cout << i << " -> ";

    for(auto edge : adj[i])
    {
        cout << "(" << edge.first << ", "
             << edge.second << ") ";
    }

    cout << endl;
}
```

For:

```text
adj[2] = [(0, 4), (3, 9)]
```

the loop:

```cpp
for(auto edge : adj[2])
```

runs as:

```text
Iteration 1:
edge = (0, 4)
edge.first = 0
edge.second = 4

Iteration 2:
edge = (3, 9)
edge.first = 3
edge.second = 9
```

### Meaning

```text
edge.first  → neighbour
edge.second → edge weight
```

Direct access:

```cpp
adj[2][0].first    // 0
adj[2][0].second   // 4
```

---

# 7. Quick Decision Table

| Graph Type            | Storage                         | Edge Insertion  |
| --------------------- | ------------------------------- | --------------- |
| Undirected Unweighted | `vector<vector<int>>`           | `u→v` and `v→u` |
| Directed Unweighted   | `vector<vector<int>>`           | `u→v`           |
| Undirected Weighted   | `vector<vector<pair<int,int>>>` | both directions |
| Directed Weighted     | `vector<vector<pair<int,int>>>` | one direction   |

## Construction Decision

```text
Weighted?
│
├── No  → store neighbour
│         vector<vector<int>>
│
└── Yes → store (neighbour, weight)
          vector<vector<pair<int,int>>>
```

Then:

```text
Undirected → insert both directions
Directed   → insert only u → v
```

---

# 8. Adjacency Matrix

For `n` nodes:

```cpp
vector<vector<int>> matrix(n, vector<int>(n, 0));
```

## Undirected Edge

```cpp
matrix[u][v] = 1;
matrix[v][u] = 1;
```

## Directed Edge

```cpp
matrix[u][v] = 1;
```

### Meaning

```text
matrix[i][j] = 1 → edge i → j exists
matrix[i][j] = 0 → edge i → j does not exist
```

### Input an Already-Given Matrix

```cpp
int n;
cin >> n;

vector<vector<int>> matrix(n, vector<int>(n));

for(int i = 0; i < n; i++)
{
    for(int j = 0; j < n; j++)
    {
        cin >> matrix[i][j];
    }
}
```

### Print Matrix

```cpp
for(int i = 0; i < n; i++)
{
    for(int j = 0; j < n; j++)
    {
        cout << matrix[i][j] << " ";
    }

    cout << endl;
}
```

### Access

```cpp
matrix[i][j]
```

---

# 9. Convert Adjacency Matrix to Adjacency List

For every `matrix[i][j] == 1`, an edge `i → j` exists.

```cpp
vector<vector<int>> adj(n);

for(int i = 0; i < n; i++)
{
    for(int j = 0; j < n; j++)
    {
        if(matrix[i][j] == 1)
        {
            adj[i].push_back(j);
        }
    }
}
```

Example matrix:

```text
1 1 0
0 1 0
1 0 1
```

Final adjacency list:

```text
adj[0] = [0, 1]
adj[1] = [1]
adj[2] = [0, 2]
```

The diagonal `1`s represent self-loops:

```text
0 → 0
1 → 1
2 → 2
```

---

# 10. Adjacency Matrix vs Grid

A matrix input can mean two different things.

### Adjacency Matrix

```text
matrix[i][j] = 1
```

means an edge exists from node `i` to node `j`.

### Grid

```text
1 1 0
0 1 0
1 0 1
```

may represent cells, land/water, blocked/open positions, etc.

In grid problems, each cell can act as a node and neighbours are generated using directions.

Do not automatically convert every matrix into an adjacency list. Read what the question says.

---

# 11. 0-Indexed vs 1-Indexed Graph

## 0-Indexed Nodes

Nodes:

```text
0 to n - 1
```

Use:

```cpp
vector<vector<int>> adj(n);
```

## 1-Indexed Nodes

Nodes:

```text
1 to n
```

Use:

```cpp
vector<vector<int>> adj(n + 1);
```

`adj[0]` remains unused.

---

# 12. Final OA Cheat Sheet

## Unweighted

```cpp
vector<vector<int>> adj(n);
```

Access:

```cpp
for(int neighbour : adj[u])
{
    // neighbour is directly connected/reachable from u
}
```

## Weighted

```cpp
vector<vector<pair<int, int>>> adj(n);
```

Access:

```cpp
for(auto edge : adj[u])
{
    int neighbour = edge.first;
    int weight = edge.second;
}
```

## Matrix

```cpp
matrix[i][j]
```

Meaning:

```text
Does edge i → j exist?
```

## Core Rule

```text
adj[u] = information about edges leaving/touching node u
```

* Unweighted → store `neighbour`
* Weighted → store `(neighbour, weight)`
* Directed → insert once
* Undirected → insert twice
