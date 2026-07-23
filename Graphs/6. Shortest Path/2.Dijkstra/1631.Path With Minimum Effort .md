# Path With Minimum Effort (LeetCode 1631)

## Problem

Given a grid of heights, move from **(0,0)** to **(rows-1, cols-1)**.

You can move:

- Up
- Down
- Left
- Right

The cost of moving between two adjacent cells is

```cpp
abs(height1 - height2)
```

The **effort of a path** is

```text
Maximum edge cost (maximum absolute height difference)
```

Return the **minimum possible effort**.

---

# Key Observation

Unlike normal shortest path,

we do **NOT** minimize

```text
Sum of edge weights
```

Instead we minimize

```text
Maximum edge weight
```

So,

```cpp
dist[r][c]
```

means

```text
Minimum effort

(minimum possible maximum jump)

from source to (r,c)
```

---

# Why Dijkstra?

Dijkstra always keeps the **best value** for every node.

Normally,

```cpp
dist = shortest distance
```

Here,

```cpp
dist = minimum effort
```

Only the relaxation changes.

---

# Relaxation

Normal Dijkstra

```cpp
newDist = currDist + weight;
```

Here

```cpp
edgeCost = abs(heights[row][col] - heights[newRow][newCol]);

newEffort = max(currEffort, edgeCost);
```

Reason:

Current path effort is the **largest jump seen so far**.

If the next jump is even larger,

that becomes the new effort.

---

# Priority Queue

Store

```text
(Effort, Row, Col)
```

Declaration

```cpp
priority_queue<
tuple<int,int,int>,
vector<tuple<int,int,int>>,
greater<tuple<int,int,int>>
> pq;
```

Push source

```cpp
pq.push({0,0,0});
```

Access

```cpp
auto [currEffort,row,col] = pq.top();
pq.pop();
```

---

# Distance Matrix

```cpp
vector<vector<int>> dist(
rows,
vector<int>(cols,INT_MAX)
);

dist[0][0]=0;
```

---

# Grid Neighbours

Unlike graph,

there is **no adjacency list**.

Generate neighbours using directions.

```cpp
int dr[] = {-1,1,0,0};
int dc[] = {0,0,-1,1};
```

Loop

```cpp
for(int i=0;i<4;i++)
{
    int newRow = row + dr[i];
    int newCol = col + dc[i];
}
```

---

# Boundary Check

```cpp
if(newRow>=0 && newRow<rows &&
   newCol>=0 && newCol<cols)
```

Only process valid cells.

---

# Edge Cost

```cpp
int edgeCost =
abs(heights[row][col] - heights[newRow][newCol]);
```

---

# Relaxation

```cpp
int newEffort = max(currEffort, edgeCost);

if(newEffort < dist[newRow][newCol])
{
    dist[newRow][newCol] = newEffort;

    pq.push({newEffort,newRow,newCol});
}
```

---

# Early Return

As soon as destination is popped,

its effort is final.

```cpp
if(row==rows-1 && col==cols-1)
    return currEffort;
```

Never compare heights.

Wrong

```cpp
if(heights[row][col]==heights[rows-1][cols-1])
```

Multiple cells can have the same height.

Always compare coordinates.

---

# Complete Algorithm

```text
Initialize dist[][]

↓

Push (0,0,0)

↓

Pop minimum effort cell

↓

Ignore stale entry

↓

If destination reached

Return effort

↓

Generate 4 neighbours

↓

Compute edge cost

↓

newEffort=max(currEffort,edgeCost)

↓

Relax

↓

Repeat
```

---

# Dry Run

Grid

```text
1 2 2
3 8 2
5 3 5
```

Start

```text
PQ

(0,0,0)
```

Pop

```text
(0,0,0)
```

Neighbours

```text
2

↓

edge=1

effort=1
```

```text
3

↓

edge=2

effort=2
```

Push

```text
(1,0,1)

(2,1,0)
```

Priority Queue always expands

```text
Lowest effort first
```

Eventually destination is popped with

```text
2
```

Answer

```text
2
```

---

# Common Mistakes

## 1. Comparing heights instead of coordinates

Wrong

```cpp
if(heights[row][col]==heights[lastRow][lastCol])
```

Correct

```cpp
if(row==lastRow && col==lastCol)
```

---

## 2. Using grid size instead of current position

Wrong

```cpp
newRow = rows + dr[i];
```

Correct

```cpp
newRow = row + dr[i];
```

---

## 3. Wrong boundary check

Wrong

```cpp
newRow < row
```

(where `row` is current row)

Correct

```cpp
newRow < rows
```

(where `rows` is total number of rows)

Use clear names like

```cpp
ROWS
COLS

currRow
currCol
```

to avoid confusion.

---

## 4. Returning before stale check

Correct order

```cpp
Pop

↓

Stale check

↓

Destination check

↓

Neighbours
```

---

## 5. Forgetting

```cpp
newEffort =
max(currEffort, edgeCost)
```

Using

```cpp
currEffort + edgeCost
```

turns it into normal Dijkstra, which is wrong.

---

# Complexity

Time

```text
O((Rows × Cols) log(Rows × Cols))
```

Space

```text
O(Rows × Cols)
```

---

# Recognition Pattern

Whenever you see

- Grid
- Path
- Minimize the **maximum** cost
- Four directions
- Non-negative costs

Think

```text
Grid Dijkstra

with

newEffort = max(currEffort, edgeCost)
```

---

# Key Takeaways

- `dist[r][c]` stores **minimum effort**, not shortest distance.
- Priority queue stores **(effort, row, col)**.
- Generate neighbours using direction arrays (`dr[]`, `dc[]`).
- Relaxation changes from `+` to `max()`.
- Stop as soon as the destination cell is popped from the priority queue.
- Use coordinate comparison for the destination, never compare cell values.
```
