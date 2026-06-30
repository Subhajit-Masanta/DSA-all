# Top View of Binary Tree

| Topic | Difficulty | Pattern                          |
| ----- | ---------- | -------------------------------- |
| Tree  | Medium     | Tree Views / Horizontal Distance |

---

## 📌 Problem Description

Given the root of a binary tree, return its **Top View**.

The **Top View** consists of the nodes visible when the tree is viewed from above.

Rules:

* Return nodes from **leftmost horizontal distance** to **rightmost horizontal distance**.
* If multiple nodes have the same horizontal distance, only the **topmost (minimum depth)** node is visible.

---

## 🧠 Intuition

Imagine drawing **vertical lines** through the tree.

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

From the **top**, only the **first node** of every column is visible.

Top View becomes

```text
4 2 1 3 7
```

---

## 🎯 Pattern Recognition

Keywords:

* Horizontal Distance (HD)
* Tree Views
* BFS
* DFS
* Map

---

## 📍 Horizontal Distance

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

BFS visits the tree

```text
Level 0

↓

Level 1

↓

Level 2
```

Therefore, the **first node** encountered for every horizontal distance is automatically the **topmost** node.

Store only the first node of every column.

---

### Algorithm

1. Push `(root, 0)` into the queue.
2. Pop one node.
3. If this column is not present in the map, store the node.
4. Push left child with `HD-1`.
5. Push right child with `HD+1`.
6. Traverse the map to build the answer.

---

## 💻 BFS Code

```cpp
class Solution {
public:

    vector<int> topView(Node *root) {

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

            if(mp.find(hd) == mp.end())
                mp[hd] = node->data;

            if(node->left)
                q.push({node->left,hd-1});

            if(node->right)
                q.push({node->right,hd+1});
        }

        for(auto t : mp)
            ans.push_back(t.second);

        return ans;
    }
};
```

---

# Approach 2 — DFS

## Why doesn't simple DFS work?

Suppose

```text
        1
       / \
      2   3
       \
        4
         \
          5
```

DFS visits

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

Column `+1`

```text
5

↓

3
```

If we store only the **first node**, we incorrectly keep **5**.

The correct answer should be **3**, because it is higher.

Therefore DFS must also remember the **level (depth)**.

---

## DFS Data Structure

```cpp
map<int, pair<int,int>>
```

Meaning

```text
Column

↓

(Node Value, Level)
```

---

### DFS Algorithm

For every node

* If column is not present

  * Store `(value, level)`.
* Else

  * Replace only if current level is smaller.

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

        if(mp.find(hd)==mp.end() || level < mp[hd].second)
            mp[hd] = {root->data,level};

        solve(root->left,level+1,hd-1);
        solve(root->right,level+1,hd+1);
    }

    vector<int> topView(Node *root){

        solve(root,0,0);

        for(auto t:mp)
            ans.push_back(t.second.first);

        return ans;
    }
};
```

---

## 🧪 Dry Run

```text
          1
        /   \
       2     3
      / \   / \
     4  5  6  7
```

Horizontal Distances

```text
4 -> -2

2 -> -1

1 -> 0

3 -> +1

7 -> +2
```

Top View

```text
4 2 1 3 7
```

---

## ⚠️ Common Mistakes

### 1. Using DFS without storing level

Wrong.

A deeper node may be visited before a shallower node.

---

### 2. Overwriting the Map in BFS

Wrong

```cpp
mp[hd] = node->data;
```

Correct

```cpp
if(mp.find(hd)==mp.end())
    mp[hd]=node->data;
```

---

### 3. Forgetting Horizontal Distance

Always update

```text
Left  → HD - 1

Right → HD + 1
```

---

## 🎯 Key Takeaways

* Horizontal Distance (HD) is the key concept.
* BFS is the preferred interview solution.
* DFS also works, but requires storing the node's level.
* One node is stored for each column.

---

## ⏱️ Complexity Analysis

| Approach | Time           | Space    |
| -------- | -------------- | -------- |
| BFS      | **O(n log n)** | **O(n)** |
| DFS      | **O(n log n)** | **O(n)** |

The `log n` factor comes from ordered `map` operations.
