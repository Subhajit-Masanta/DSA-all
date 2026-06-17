# Rat in a Maze

| Topic         | Difficulty | Pattern           |
| :------------ | :--------- | :---------------- |
| Rat in a Maze | Medium     | Grid Backtracking |

---

# Problem

Given an `n × n` binary matrix,

```
1 = Open Cell

0 = Blocked Cell
```

Find **all possible paths** from

```
(0,0)

↓

(n-1,n-1)
```

Allowed moves

```
Up (U)

Down (D)

Left (L)

Right (R)
```

A cell cannot be visited twice in the same path.

---

# Core Idea

Word Search

```
Need to match

ABCCED

↓

Return true / false
```

Rat in Maze

```
Need to reach destination

↓

Store every valid path

↓

Return vector<string>
```

---

# What do we maintain?

```
row

col

currentPath
```

Example

```
currentPath="DDR"
```

Current position reached after

```
Down

Down

Right
```

---

# Base Cases

## Destination reached

```cpp
if(row==rows-1 && col==cols-1)
{
    ans.push_back(current);
    return;
}
```

---

## Out of Boundary

```cpp
if(row<0 || row>=rows ||
   col<0 || col>=cols)
    return;
```

---

## Blocked / Visited Cell

```cpp
if(maze[row][col]!=1)
    return;
```

---

# Mark Visited

Current

```
1 1

1 1
```

Visit

```
(0,0)
```

```
1

↓

0 (or #)
```

```cpp
temp=maze[row][col];

maze[row][col]=0;
```

---

# Four Choices

Question requires lexicographical order.

Explore

```
Down

Left

Right

Up
```

---

# Backtracking Pattern

Every direction follows

```
Choose

↓

Explore

↓

Undo
```

```cpp
current+='D';

solve(...);

current.pop_back();
```

---

# Dry Run

Maze

```
1 1

1 1
```

Start

```
current=""
```

Choose Down

```
current="D"
```

Choose Right

```
current="DR"
```

Destination reached

```
ans.push_back("DR")
```

Return

```
current.pop_back()

↓

"D"
```

Return

```
current.pop_back()

↓

""
```

Now try Right branch independently.

---

# Recursion Tree

```
                 ""

          /    /   |   \

         D     L   R    U

         |

        "D"

         |

        "DR"

         |

      Destination

         |

      Store Answer
```

---

# Code

```cpp
class Solution {
public:

    void solve(int row,int col,
               vector<vector<int>>& maze,
               vector<string>& ans,
               string& current)
    {
        int rows=maze.size();
        int cols=maze[0].size();

        if(row==rows-1 && col==cols-1)
        {
            ans.push_back(current);
            return;
        }

        if(row<0 || row>=rows ||
           col<0 || col>=cols)
            return;

        if(maze[row][col]!=1)
            return;

        int temp=maze[row][col];
        maze[row][col]=0;

        current+='D';
        solve(row+1,col,maze,ans,current);
        current.pop_back();

        current+='L';
        solve(row,col-1,maze,ans,current);
        current.pop_back();

        current+='R';
        solve(row,col+1,maze,ans,current);
        current.pop_back();

        current+='U';
        solve(row-1,col,maze,ans,current);
        current.pop_back();

        maze[row][col]=temp;
    }

    vector<string> ratInMaze(vector<vector<int>>& maze)
    {
        vector<string> ans;
        string current="";

        if(maze[0][0]==0)
            return ans;

        solve(0,0,maze,ans,current);

        return ans;
    }
};
```

---

# Time Complexity

Worst Case

```
O(4^(N×N))
```

Each cell can explore four directions.

---

# Space Complexity

```
O(N×N)
```

Maximum recursion depth.

---

# Common Mistakes

## 1. Wrong Destination Check

❌

```cpp
row==row-1
```

✔

```cpp
row==rows-1 &&
col==cols-1
```

---

## 2. Starting From Every Cell

❌

```cpp
for(i)
    for(j)
        solve(...)
```

Rat always starts from

```
(0,0)
```

---

## 3. Forgetting Restore

```cpp
maze[row][col]=temp;
```

Without restore, future branches think the cell is permanently blocked.

---

## 4. Forgetting pop_back()

Every branch must start with a clean path.

Correct pattern

```
+D

↓

solve

↓

pop

↓

+L

↓

solve

↓

pop
```

---

# Discussion

## Why Restore?

```
Choose Cell

↓

Mark Visited

↓

Explore

↓

Restore

↓

Another branch can reuse it
```

Exactly like

```
push()

↓

solve()

↓

pop()
```

in Combination Sum.

---

## Can we use

```cpp
solve(...,current+'D');
```

YES.

Then

```cpp
string current
```

(pass by value)

instead of

```cpp
string& current
```

No

```cpp
pop_back()
```

needed.

---

## Reference vs Copy

### Reference

```cpp
current+='D';

solve(...);

current.pop_back();
```

Pattern

```
Choose

↓

Explore

↓

Undo
```

Uses one string.

---

### Copy

```cpp
solve(...,current+'D');
```

Creates

```
New String

↓

Explore

↓

Destroyed automatically
```

Simpler but creates many copies.

---

# Pattern Recognition

```
Generate Parentheses

↓

Subsets

↓

Combination Sum

↓

Permutations

↓

Letter Combinations

↓

Palindrome Partitioning

↓

Word Search

↓

⭐⭐ Rat in a Maze ⭐⭐

(Current Cell + Current Path)

↓

Choose

↓

Explore

↓

Undo
```

---

# Interview One-Liner

> Rat in a Maze is Grid Backtracking where we maintain the current path, mark cells as visited, explore four directions in lexicographical order, restore the cell, and undo the path using `pop_back()` before exploring the next branch.
