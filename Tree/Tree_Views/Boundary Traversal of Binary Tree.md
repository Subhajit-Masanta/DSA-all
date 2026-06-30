# Boundary Traversal of Binary Tree

| Topic | Difficulty | Pattern                  |
| ----- | ---------- | ------------------------ |
| Tree  | Medium     | Boundary Traversal / DFS |

---

# 📌 Problem Description

Given the root of a binary tree, return its **Boundary Traversal** in **anti-clockwise** order.

The traversal consists of four parts:

1. Root
2. Left Boundary (excluding leaf nodes)
3. All Leaf Nodes (left to right)
4. Right Boundary in **reverse order** (excluding leaf nodes)

---

# 🧠 Intuition

Imagine walking around the outside of the tree in an anti-clockwise direction.

Example

```text
          1
       /     \
      2       3
     / \     / \
    4   5   6   7
       / \
      8   9
```

Walking around the outside gives

```text
1

↓

2

↓

4

↓

8

↓

9

↓

6

↓

7

↓

3
```

Answer

```text
[1,2,4,8,9,6,7,3]
```

---

# Pattern

This is **NOT** a single traversal.

It is the combination of **three DFS traversals**.

```text
Root

↓

Left Boundary

↓

All Leaves

↓

Right Boundary (Reverse)
```

---

# Why Exclude Leaf Nodes?

Suppose

```text
      1
     /
    2
   /
  3
```

If left boundary includes leaves

```text
1 2 3
```

and leaf traversal also adds

```text
3
```

Final answer becomes

```text
1 2 3 3
```

Wrong.

Therefore

* Left Boundary → Exclude leaves
* Right Boundary → Exclude leaves
* Leaf Traversal → Adds every leaf exactly once.

---

# Helper Function

```cpp
bool isLeaf(Node* root)
{
    return root->left == nullptr &&
           root->right == nullptr;
}
```

Used by all three traversals.

---

# Left Boundary

Rule

```text
Keep moving

Left

↓

Else Right
```

Stop before leaf nodes.

Example

```text
        1
       /
      2
       \
        3
```

Left Boundary

```text
2

↓

3
```

because there is no left child.

---

# Leaf Traversal

Simple DFS.

Whenever

```cpp
isLeaf(root)
```

is true,

store the node.

Leaves are automatically collected from **left to right**.

---

# Right Boundary

Rule

```text
Keep moving

Right

↓

Else Left
```

Again,

ignore leaves.

Store nodes inside a temporary vector.

Reverse it before adding to the answer.

---

# Algorithm

```text
Create ans

↓

If root is not leaf

↓

Add root

↓

Left Boundary

↓

Leaf Nodes

↓

Right Boundary

↓

Reverse Right Boundary

↓

Append to ans

↓

Return ans
```

---

# Code

```cpp
class Solution {
public:

    vector<int> ans;
    vector<int> rig;

    bool isLeaf(Node* root)
    {
        return root->left == nullptr &&
               root->right == nullptr;
    }

    void left(Node* root)
    {
        if(root == nullptr || isLeaf(root))
            return;

        ans.push_back(root->data);

        if(root->left)
            left(root->left);
        else
            left(root->right);
    }

    void leaf(Node* root)
    {
        if(root == nullptr)
            return;

        if(isLeaf(root))
        {
            ans.push_back(root->data);
            return;
        }

        leaf(root->left);
        leaf(root->right);
    }

    void right(Node* root)
    {
        if(root == nullptr || isLeaf(root))
            return;

        rig.push_back(root->data);

        if(root->right)
            right(root->right);
        else
            right(root->left);
    }

    vector<int> boundaryTraversal(Node* root)
    {
        if(root == nullptr)
            return {};

        if(!isLeaf(root))
            ans.push_back(root->data);

        left(root->left);

        leaf(root);

        right(root->right);

        reverse(rig.begin(), rig.end());

        for(auto x : rig)
            ans.push_back(x);

        return ans;
    }
};
```

---

# Dry Run

```text
          1
       /     \
      2       3
     / \     / \
    4   5   6   7
       / \
      8   9
```

## Step 1

Root

```text
1
```

Answer

```text
1
```

---

## Step 2

Left Boundary

```text
2
```

Answer

```text
1 2
```

---

## Step 3

Leaves

```text
4

8

9

6

7
```

Answer

```text
1 2 4 8 9 6 7
```

---

## Step 4

Right Boundary

Collected

```text
3
```

Reverse

```text
3
```

Final Answer

```text
1 2 4 8 9 6 7 3
```

---

# Common Mistakes

## ❌ Including leaf nodes inside left/right boundary

Results in duplicate leaves.

---

## ❌ Forgetting reverse()

Right Boundary must be added from **bottom to top**.

---

## ❌ Always going left

Correct rule

```text
Left Boundary

Left

↓

Else Right
```

---

## ❌ Always going right

Correct rule

```text
Right Boundary

Right

↓

Else Left
```

---

## ❌ Adding root twice

If the tree has only one node

```text
1
```

Do

```cpp
if(!isLeaf(root))
    ans.push_back(root->data);
```

Otherwise the root will also be added during leaf traversal.

---

# Complexity

| Operation | Complexity  |
| --------- | ----------- |
| Time      | O(n)        |
| Space     | O(h) + O(n) |

where

* **h** = height of tree (recursion stack)
* **n** = nodes stored in answer

---

# Key Takeaways

* Boundary Traversal is **three separate DFS traversals**.
* Left Boundary excludes leaves.
* Leaf Traversal collects every leaf exactly once.
* Right Boundary excludes leaves and is added in reverse.
* `isLeaf()` simplifies the implementation and prevents duplicates.
* This is one of the most common tree traversal interview patterns.
