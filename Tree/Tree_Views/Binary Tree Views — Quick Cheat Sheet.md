# 🌳 Binary Tree Views — Quick Cheat Sheet

## Common Coordinate Rules

```text
Root  → level = 0, hd = 0

Left  → level + 1, hd - 1
Right → level + 1, hd + 1
```

Example:

```text
              1
           hd=0,l=0
            /     \
           2       3
       hd=-1      hd=+1
        /  \       /  \
       4    5     6    7
    hd=-2  hd=0  hd=0  hd=+2
```

---

# 1. Top View

## Idea

For every horizontal distance `hd`, keep the **highest node**.

```text
hd → node with minimum level
```

## Store

```cpp
map<int, pair<int,int>> mp;

// hd → {value, level}
```

## Rule

```cpp
if (mp.find(hd) == mp.end() || level < mp[hd].second) {
    mp[hd] = {root->data, level};
}
```

## Example

```text
hd:    -2  -1   0  +1  +2
Top:    4   2   1   3   7
```

`map` automatically sorts `hd`:

```text
left → right
```

### Remember

```text
TOP = minimum level at each hd
```

---

# 2. Bottom View

## Idea

For every horizontal distance `hd`, keep the **lowest node**.

```text
hd → node with maximum level
```

## Store

```cpp
map<int, pair<int,int>> mp;

// hd → {value, level}
```

## Rule

```cpp
if (mp.find(hd) == mp.end() || level >= mp[hd].second) {
    mp[hd] = {root->data, level};
}
```

## Example

```text
hd:       -2  -1   0  +1  +2
Bottom:    4   2   6   3   7
```

### Remember

```text
BOTTOM = maximum level at each hd
```

---

# 3. Left View

## Idea

Take the **first node seen at every level** while going left first.

## Store

```cpp
vector<int> ans;
```

## DFS Order

```text
Root → Left → Right
```

## Rule

```cpp
if (level == ans.size()) {
    ans.push_back(root->data);
}
```

## Example

```text
Level:  0  1  2
Left:   1  2  4
```

### Remember

```text
LEFT VIEW = first node of each level
            using left-first DFS
```

---

# 4. Right View

## Idea

Take the **first node seen at every level** while going right first.

## Store

```cpp
vector<int> ans;
```

## DFS Order

```text
Root → Right → Left
```

## Rule

```cpp
if (level == ans.size()) {
    ans.push_back(root->data);
}
```

## Example

```text
Level:  0  1  2
Right:  1  3  7
```

### Remember

```text
RIGHT VIEW = first node of each level
             using right-first DFS
```

---

# 5. Vertical Traversal

## Idea

Group nodes by:

```text
Column → Row → Values
```

## Store

```cpp
map<int, map<int, multiset<int>>> mp;

// column → row → sorted values
```

## Insert

```cpp
mp[column][row].insert(root->val);
```

## Sorting

```text
Outer map  → columns sorted left to right
Inner map  → rows sorted top to bottom
Multiset   → same position values sorted
```

## Example Storage

```text
Column -2
└── Row 2 → {4}

Column -1
└── Row 1 → {2}

Column 0
├── Row 0 → {1}
└── Row 2 → {5, 6}

Column +1
└── Row 1 → {3}

Column +2
└── Row 2 → {7}
```

Result:

```text
[
    [4],
    [2],
    [1, 5, 6],
    [3],
    [7]
]
```

### Remember

```text
VERTICAL = column → row → sorted values
```

---

# 🔥 Final Comparison

| View | Group By | What to Keep |
|---|---|---|
| Top View | `hd` | Minimum level |
| Bottom View | `hd` | Maximum level |
| Left View | Level | First node, left-first DFS |
| Right View | Level | First node, right-first DFS |
| Vertical Traversal | Column + Row | Sorted columns, rows and values |

---

# 🧠 One-Line Memory Trick

```text
TOP      → highest node at each hd

BOTTOM   → lowest node at each hd

LEFT     → first node at each level (go left first)

RIGHT    → first node at each level (go right first)

VERTICAL → column → row → sorted values
```

---

# Container Cheat Sheet

```cpp
// Top View
map<int, pair<int,int>>
// hd → {value, level}


// Bottom View
map<int, pair<int,int>>
// hd → {value, level}


// Left View
vector<int>
// one value per level


// Right View
vector<int>
// one value per level


// Vertical Traversal
map<int, map<int, multiset<int>>>
// column → row → sorted values
```
