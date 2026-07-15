# 207. Course Schedule

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
You must complete course b

↓

Before taking course a
```

Question

```text
Can you finish ALL courses?
```

Return

```text
true
```

if possible.

Otherwise

```text
false
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

Possible Order

```text
0

↓

1
```

Return

```text
true
```

---

# Example 2

Input

```cpp
[[1,0],[0,1]]
```

Graph

```text
0 → 1
↑   |
|   |
+---+
```

Course 0 waits for 1.

Course 1 waits for 0.

Impossible.

Return

```text
false
```

---

# 🧠 Intuition

This is exactly a

```text
Topological Sort
```

problem.

Think of

```text
Indegree
```

as

```text
Remaining Prerequisites
```

Initially,

all courses with

```text
indegree = 0
```

can be taken immediately.

After finishing one course,

its dependent courses lose one prerequisite.

If a course's indegree becomes

```text
0
```

it is now ready.

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

because

```text
Finish b first

↓

Then a
```

Graph

```cpp
adj[b].push_back(a);
```

Indegree

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

Push all indegree 0 nodes

↓

While Queue not Empty

↓

Pop Course

↓

processed++

↓

Visit neighbours

↓

Decrease their indegree

↓

If indegree becomes 0

↓

Push into Queue

↓

After BFS

↓

processed == numCourses ?

↓

Yes → true

No → false
```

---

# Code

```cpp
class Solution {
public:

    bool canFinish(int numCourses,
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

        int processed=0;

        while(!q.empty())
        {
            int node=q.front();
            q.pop();

            processed++;

            for(int neighbour : adj[node])
            {
                indegree[neighbour]--;

                if(indegree[neighbour]==0)
                    q.push(neighbour);
            }
        }

        return processed==numCourses;
    }
};
```

---

# Dry Run

Input

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

Processed

```text
1
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

Decrease

```text
3 : 2 → 1
```

---

Pop

```text
2
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

Processed

```text
4
```

Since

```text
processed == numCourses
```

Return

```text
true
```

---

# How Cycle is Detected

Example

```text
0 → 1

1 → 2

2 → 1
```

Initial Indegree

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

because they depend on each other.

Processed

```text
1
```

Total Courses

```text
3
```

Since

```text
processed != numCourses
```

Return

```text
false
```

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

Correct

```cpp
indegree[course]++;
```

---

## ❌ Returning if Queue is Empty Initially

Do not return immediately.

Always run Kahn's Algorithm and compare

```cpp
processed == numCourses
```

---

# Complexity

| Complexity | Value |
|------------|-------|
| Time | **O(V + E)** |
| Auxiliary Space | **O(V + E)** |

---

# Difference from GFG Topological Sort

| GFG Topological Sort | Course Schedule |
|----------------------|-----------------|
| Return ordering | Return true/false |
| Store topo vector | Count processed nodes |
| Output vector | Output boolean |

Algorithm is exactly the same.

---

# Recognition Pattern ⭐⭐⭐⭐⭐

Whenever the problem mentions

```text
Courses

Prerequisites

Dependencies

Scheduling

Tasks

Must complete first
```

Think

```text
Directed Graph

↓

Topological Sort

↓

Kahn's Algorithm

↓

processed == total nodes ?
```

---

# Key Takeaways

- Convert prerequisites into a directed graph.
- Edge direction: **Prerequisite → Course**.
- `indegree` = remaining prerequisites.
- Start with all courses having indegree **0**.
- Process courses using Kahn's Algorithm.
- If all courses are processed, return `true`.
- If some courses remain, a cycle exists, so return `false`.
