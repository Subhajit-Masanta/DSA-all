# Bottom View of Binary Tree

| Topic | Difficulty | Pattern                          |
| ----- | ---------- | -------------------------------- |
| Tree  | Medium     | Tree Views / Horizontal Distance |

---

# 📌 Problem Description

Given the root of a binary tree, return its **Bottom View**.

The Bottom View consists of the nodes visible when the tree is viewed from **below**.

Rules:

* Return nodes from **leftmost** horizontal distance (HD) to **rightmost**.
* If multiple nodes exist at the same HD, the **bottom-most** node should be chosen.
* If two nodes have the **same HD and same depth**, choose the one encountered **later in level-order traversal**.

---

# 🧠 Intuition

Imagine drawing vertical lines through the tree.

```text
Column -2

|

4

----------------

Column -1

|

2

----------------

Column 0

|

1
5
6

----------------

Column 1

|

3

----------------

Column 2

|

7
```

From the **bottom**, only the **last visible node** of every column is seen.

---

# Horizontal Distance (HD)

Root

```text
HD = 0
```

Left Child

```text
HD - 1
```

Right Child

```text
HD + 1
```

Example

```text
        1(0)
      /      \
   2(-1)     3(+1)
   /             \
4(-2)           7(+2)
```

---

# Approach 1 — BFS (Recommended)

## Why BFS?

BFS processes

```text
Level 0

↓

Level 1

↓

Level 2

↓

Level 3
```

Since deeper nodes are processed later, simply **overwriting** the value at each HD naturally leaves the bottom-most node.

---

## Algorithm

1. Push `(root, 0)` into the queue.
2. Pop a node.
3. Store

```cpp
mp[hd] = node->data;
```

4. Push left child with `HD-1`.
5. Push right child with `HD+1`.
6. Traverse the map.

---

## 💻 BFS Code

```cpp
class Solution {
public:

    vector<int> bottomView(Node *root) {

        vector<int> ans;

        if(root == nullptr)
            return ans;

        map<int,int> mp;

        queue<pair<Node*,int>> q;

        q.push({root,0});

        while(!q.empty()){

            auto t = q.front();
            q.pop();

            Node* node = t.first;
            int hd = t.second;

            mp[hd] = node->data;

            if(node->left)
                q.push({node->left,hd-1});

            if(node->right)
                q.push({node->right,hd+1});
        }

        for(auto t:mp)
            ans.push_back(t.second);

        return ans;
    }
};
```

---

# Approach 2 — DFS

## Why isn't simple DFS enough?

Consider

```text
        1
       / \
      2   3
       \
        4
         \
          5
```

DFS order

```text
1

↓

2

↓

4

↓

5

↓

3
```

At

```text
HD = +1
```

DFS visits

```text
5

↓

3
```

If we simply overwrite

```cpp
mp[hd] = value;
```

we incorrectly keep **3**.

The correct Bottom View is **5**, because it is deeper.

Therefore DFS must store **depth (level)** as well.

---

# DFS Data Structure

```cpp
map<int,pair<int,int>>
```

Meaning

```text
HD

↓

(Value, Level)
```

---

## DFS Algorithm

For every node

* If HD is not present

  * Store `(value, level)`
* Else

  * Replace only if

```text
Current Level >= Stored Level
```

---

## 💻 DFS Code

```cpp
class Solution {
public:

    map<int,pair<int,int>> mp;
    vector<int> ans;

    void solve(Node* root,int level,int hd){

        if(root == nullptr)
            return;

        if(mp.find(hd)==mp.end() || level >= mp[hd].second)
            mp[hd] = {root->data,level};

        solve(root->left,level+1,hd-1);
        solve(root->right,level+1,hd+1);
    }

    vector<int> bottomView(Node *root){

        solve(root,0,0);

        for(auto t:mp)
            ans.push_back(t.second.first);

        return ans;
    }
};
```

---

# Top View vs Bottom View

| Feature | Top View           | Bottom View        |
| ------- | ------------------ | ------------------ |
| BFS     | Store first node   | Store last node    |
| DFS     | Keep minimum level | Keep maximum level |

---

# Common Mistakes

### ❌ DFS without storing level

Wrong.

DFS traversal order is **not** the same as depth order.

---

### ❌ Returning inside loop

Wrong

```cpp
for(auto t:mp){
    ans.push_back(t.second);
    return ans;
}
```

Return after the loop.

---

### ❌ Wrong HD Update

Always

```text
Left  → HD - 1

Right → HD + 1
```

---

# Key Takeaways

* Horizontal Distance (HD) is the core concept.
* BFS is the preferred interview solution.
* DFS also works but must store `(value, level)`.
* Bottom View keeps the **deepest** node at every HD.
* In BFS, simply overwrite the map entry.

---

# Complexity

| Approach | Time       | Space |
| -------- | ---------- | ----- |
| BFS      | O(n log n) | O(n)  |
| DFS      | O(n log n) | O(n)  |

The `log n` factor comes from ordered `map` operations.
