# Binary Search Tree (BST) - Introduction

---

# What is a Binary Search Tree?

A **Binary Search Tree (BST)** is a Binary Tree that follows a special ordering rule.

For every node:

```text
Left Subtree < Root < Right Subtree
```

This property must hold **for every node** in the tree, not just the root.

---

# BST Property

```text
              8
            /   \
           3     10
          / \      \
         1   6      14
            / \     /
           4   7   13
```

For node **8**

```text
Left subtree values

1 3 4 6 7

↓

All < 8

----------------

Right subtree values

10 13 14

↓

All > 8
```

For node **3**

```text
1

↓

< 3

---------------

4 6 7

↓

> 3
```

Notice that the rule applies **recursively** to every subtree.

---

# Binary Tree vs BST

| Binary Tree                               | Binary Search Tree                 |
| ----------------------------------------- | ---------------------------------- |
| No ordering property                      | Left < Root < Right                |
| Search may require visiting both subtrees | Search follows only one path       |
| Traversal order depends on tree           | Inorder traversal is always sorted |

---

# Most Important Property

## ⭐ Inorder Traversal of a BST is Always Sorted

Example

```text
              5
            /   \
           3     7
          / \   / \
         2   4 6   8
```

Inorder

```text
2 3 4 5 6 7 8
```

This single property is used in many interview questions:

* Validate BST
* Kth Smallest
* BST Iterator
* Minimum Difference in BST
* Recover BST

---

# Searching in a BST

Suppose we want to find

```text
7
```

Tree

```text
          8
        /   \
       3     10
      / \
     1   6
        / \
       4   7
```

Compare

```text
7 < 8

↓

Go Left
```

Now

```text
7 > 3

↓

Go Right
```

Now

```text
7 > 6

↓

Go Right
```

Found

```text
7
```

Notice we never searched the whole tree.

---

# Why is BST Fast?

Because each comparison eliminates roughly half of the remaining search space (when the tree is balanced).

Average Complexity

| Operation | Time     |
| --------- | -------- |
| Search    | O(log n) |
| Insert    | O(log n) |
| Delete    | O(log n) |

Worst Case (Skewed Tree)

| Operation | Time |
| --------- | ---- |
| Search    | O(n) |
| Insert    | O(n) |
| Delete    | O(n) |

A skewed BST behaves like a linked list.

---

# Balanced vs Skewed BST

Balanced

```text
        4
      /   \
     2     6
    / \   / \
   1  3  5  7
```

Height

```text
O(log n)
```

---

Skewed

```text
1
 \
  2
   \
    3
     \
      4
```

Height

```text
O(n)
```

Searching becomes linear.

---

# Mental Model

Whenever you solve a BST problem, ask:

```text
Can I use the BST property?

↓

Left < Root < Right
```

If yes,

do **not** traverse the whole tree.

Instead,

decide

```text
Target < Root ?

↓

Go Left

----------------

Target > Root ?

↓

Go Right
```

Only one recursive call or one loop iteration is needed at each step.

---

# Common Interview Patterns

## 1. Search

Use BST property.

Move only left or right.

---

## 2. Insert

Search until a null position.

Insert new node.

---

## 3. Delete

Handle

* Leaf node
* One child
* Two children

---

## 4. Validate BST

Check that every node lies within a valid range.

---

## 5. Kth Smallest

Use the fact that

```text
Inorder = Sorted Order
```

---

## 6. Lowest Common Ancestor

Compare both values with the current node.

If both are smaller → go left.

If both are larger → go right.

Otherwise, current node is the answer.

---

# Beginner Mistakes

❌ Checking only

```text
left < root

right > root
```

This is **not enough**.

Every node in the left subtree must be smaller, and every node in the right subtree must be larger.

---

❌ Forgetting that

```text
Inorder Traversal

↓

Sorted
```

This is the most important BST property.

---

❌ Traversing both subtrees while searching.

Use the BST property.

Only one direction is explored.

---

# Problems Roadmap

1. 700. Search in a BST
2. 98. Validate BST
3. 230. Kth Smallest Element in a BST
4. 235. Lowest Common Ancestor of a BST
5. 701. Insert into a BST
6. 450. Delete Node in a BST

---

# Key Takeaways

* BST is a Binary Tree with the property **Left < Root < Right**.
* The property is valid for every subtree.
* Inorder traversal always produces sorted values.
* Search, Insert and Delete are **O(log n)** on average for balanced BSTs.
* Always try to use the BST property before thinking about DFS or BFS.
