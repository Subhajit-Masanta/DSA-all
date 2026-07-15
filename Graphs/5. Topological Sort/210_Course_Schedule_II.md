# 210. Course Schedule II

| Topic | Difficulty | Pattern |
|--------|------------|---------|
| Graph | Medium | Kahn's Algorithm (Topological Sort) |

---

# 📌 Problem Description

There are

```text
numCourses
```

courses numbered

```text
0 ... numCourses-1
```

Each prerequisite

```cpp
[a, b]
```

means

```text
Finish course b

↓

Before taking course a
```

Return

```text
One valid order
```

to complete all courses.

If it is impossible (cycle exists),

return

```text
{}
```

---

# Example 1

Input

```cpp
numCourses = 2

prerequisites = [[1,0]]
```

Graph

```text
0 → 1
```

Answer

```text
0 1
```

---

# Example 2

Input

```cpp
numCourses = 2

prerequisites = [[1,0],[0,1]]
```

Graph

```text
0 → 1
↑   |
|   |
+---+
```

Cycle exists.

Answer

```text
{}
```

---

# 🧠 Intuition

This problem is almost identical to

```text
207. Course Schedule
```

Difference

207

```text
Return true / false
```

210

```text
Return the Topological Order
```

If all courses are processed,

the answer vector itself is the required order.

Otherwise,

return an empty vector.

---

# Graph Construction

Input

```cpp
[a,b]
```

means

```text
b → a
```

Build graph

```cpp
adj[b].push_back(a);
```

Increase indegree

```cpp
indegree[a]++;
```

---

# Algorithm

```text
Build Directed Graph

↓

Compute Indegree

↓

Push every node with indegree = 0

↓

While Queue not Empty

↓

Pop Node

↓

Store in Answer

↓

Visit neighbours

↓

Decrease their indegree

↓

Neighbour indegree becomes 0 ?

↓

Push into Queue

↓

Finished

↓

Answer size == numCourses ?

↓

Yes → Return Answer

No → Return {}
```

---

# Code

```cpp
class Solution {
public:

    vector<int> findOrder(int numCourses,
                          vector<vector<int>>& prerequisites)
    {
        vector<vector<int>> adj(numCourses);

        vector<int> indegree(numCourses,0);

        for(auto edge : prerequisites)
        {
            int course = edge[0];
            int prerequisite = edge[1];

            indegree[course]++;

            adj[prerequisite].push_back(course);
        }

        queue<int> q;

        for(int i=0;i<numCourses;i++)
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

            for(int neighbour : adj[node])
            {
                indegree[neighbour]--;

                if(indegree[neighbour]==0)
                    q.push(neighbour);
            }
        }

        if(ans.size()==numCourses)
            return ans;

        return {};
    }
};
```

---

# Dry Run

Graph

```text
0 → 1

0 → 2

1 → 3

2 → 3
```

Initial Indegree

```text
0 : 0

1 : 1

2 : 1

3 : 2
```

Queue

```text
0
```

---

Pop

```text
0
```

Answer

```text
0
```

Decrease

```text
1 : 1 → 0

2 : 1 → 0
```

Push

```text
1

2
```

Queue

```text
1

2
```

---

Pop

```text
1
```

Answer

```text
0 1
```

Decrease

```text
3 : 2 → 1
```

---

Pop

```text
2
```

Answer

```text
0 1 2
```

Decrease

```text
3 : 1 → 0
```

Push

```text
3
```

---

Pop

```text
3
```

Answer

```text
0 1 2 3
```

Since

```text
ans.size() == numCourses
```

Return

```text
0 1 2 3
```

---

# Cycle Example

Graph

```text
0 → 1

1 → 0
```

Initial Indegree

```text
0 : 1

1 : 1
```

Queue

```text
Empty
```

No node can be processed.

Answer

```text
{}
```

Since

```text
ans.size() != numCourses
```

Return

```text
{}
```

---

# Difference from 207

| 207 | 210 |
|------|------|
| Return `bool` | Return `vector<int>` |
| Count processed nodes | Store processed nodes |
| `processed == numCourses` | `ans.size() == numCourses` |

Everything else is identical.

---

# Common Mistakes

## ❌ Wrong Edge Direction

Wrong

```cpp
adj[course].push_back(prerequisite);
```

Correct

```cpp
adj[prerequisite].push_back(course);
```

---

## ❌ Wrong Indegree

Increase indegree of

```text
Course
```

not

```text
Prerequisite
```

---

## ❌ Returning Answer Directly

Always check

```cpp
ans.size() == numCourses
```

Otherwise a cyclic graph may return a partial ordering.

---

# Complexity

| Complexity | Value |
|------------|-------|
| Time | **O(V + E)** |
| Auxiliary Space | **O(V + E)** |

---

# Recognition Pattern ⭐⭐⭐⭐⭐

Whenever the problem asks

```text
Return one valid order

Scheduling

Dependencies

Prerequisites

Execution Order
```

Think

```text
Directed Graph

↓

Topological Sort

↓

Kahn's Algorithm
```

---

# Key Takeaways

- Same algorithm as **Course Schedule (207)**.
- Difference is only the return type.
- Store nodes in the order they are processed.
- If all nodes are processed, that order is valid.
- If some nodes remain, a cycle exists, so return `{}`.
