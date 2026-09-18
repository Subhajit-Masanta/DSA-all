# Session 3 — BST + Recursion

**Revision time:** 45–60 minutes

## BST Rule

For every node:

```text
Left < Root < Right
```

Most BST operations follow one path from the root, so their cost is **O(h)** where `h` is the tree height. For a balanced BST, `h = O(log n)`; for a skewed BST, `h = O(n)`.

---

# 1. Height of Binary Tree

## Idea

Height =

```text
1 + maximum(left height, right height)
```

Base case:

```text
nullptr → 0
```

## Code

```cpp
int height(TreeNode* root)
{
    if(root == nullptr)
        return 0;

    int left = height(root->left);
    int right = height(root->right);

    return 1 + max(left, right);
}
```

## Example

```text
        1
       / \
      2   3
     /
    4
```

```text
height(4) = 1

height(2) = 1 + max(1,0)
          = 2

height(3) = 1

height(1) = 1 + max(2,1)
          = 3
```

### Complexity

```text
Time  : O(n)
Space : O(h)
```

Every node is visited once. Recursion uses the height of the tree.

---

# 2. Count Nodes

## Idea

Every node contributes `1`.

```text
count(root)
=
1
+
count(left)
+
count(right)
```

## Code

```cpp
int countNodes(TreeNode* root)
{
    if(root == nullptr)
        return 0;

    int left = countNodes(root->left);
    int right = countNodes(root->right);

    return 1 + left + right;
}
```

## Example

```text
        1
       / \
      2   3
     /
    4
```

```text
count(4) = 1

count(2) = 1 + 1 + 0
         = 2

count(3) = 1

count(1) = 1 + 2 + 1
         = 4
```

### Complexity

```text
Time  : O(n)
Space : O(h)
```

Every node must be visited.

---

# 3. Search in BST — LeetCode 700

## Key Idea

Use the BST property.

```text
val < root->val
        ↓
      LEFT

val > root->val
        ↓
      RIGHT
```

Do **not** search both sides.

## Recursive Code

```cpp
TreeNode* searchBST(TreeNode* root, int val)
{
    if(root == nullptr)
        return nullptr;

    if(root->val == val)
        return root;

    if(val < root->val)
        return searchBST(root->left, val);

    return searchBST(root->right, val);
}
```

## Example

```text
        8
       / \
      3   10
     / \
    1   6
       / \
      4   7
```

Search `7`.

```text
7 < 8
↓
LEFT

7 > 3
↓
RIGHT

7 > 6
↓
RIGHT

7 == 7
↓
FOUND
```

### Complexity

```text
Time  : O(h)
Space : O(h) recursive
```

Balanced:

```text
O(log n)
```

Worst-case skewed:

```text
O(n)
```

An iterative implementation uses `O(1)` auxiliary space.

---

# 4. Insert into BST — LeetCode 701

## Key Idea

Insertion follows exactly the same path as search.

Difference:

### Search

When we reach `nullptr`:

```text
return nullptr
```

### Insert

When we reach `nullptr`:

```text
create new node
```

## Code

```cpp
TreeNode* insertIntoBST(TreeNode* root, int val)
{
    if(root == nullptr)
        return new TreeNode(val);

    if(val < root->val)
        root->left = insertIntoBST(root->left, val);
    else
        root->right = insertIntoBST(root->right, val);

    return root;
}
```

## Example

Insert `25`.

```text
        40
       /  \
     20    60
       \
       30
```

### Step 1

```text
25 < 40
↓
left
```

### Step 2

```text
25 > 20
↓
right
```

### Step 3

```text
25 < 30
↓
left
```

### Step 4

```text
30->left == nullptr
```

Create:

```text
25
```

Final:

```text
        40
       /  \
     20    60
       \
       30
       /
      25
```

### Complexity

```text
Time  : O(h)
Space : O(h) recursive
```

Balanced:

```text
O(log n)
```

Worst case:

```text
O(n)
```

Insertion follows one root-to-leaf path.

---

# 5. Minimum in BST

## Key Idea

The smallest value is always the **leftmost node**.

Why?

```text
Left < Root
```

So keep going left.

## Code

```cpp
TreeNode* minimum(TreeNode* root)
{
    if(root == nullptr)
        return nullptr;

    while(root->left)
        root = root->left;

    return root;
}
```

Example:

```text
        50
       /  \
     30    70
    /
   20
  /
 10
```

Minimum:

```text
50
 ↓
30
 ↓
20
 ↓
10
```

Answer:

```text
10
```

### Complexity

```text
Time  : O(h)
Space : O(1)
```

---

# 6. Maximum in BST

## Key Idea

The largest value is always the **rightmost node**.

Keep going right.

## Code

```cpp
TreeNode* maximum(TreeNode* root)
{
    if(root == nullptr)
        return nullptr;

    while(root->right)
        root = root->right;

    return root;
}
```

Example:

```text
        50
       /  \
     30    70
             \
              90
               \
                100
```

Maximum:

```text
50
 ↓
70
 ↓
90
 ↓
100
```

Answer:

```text
100
```

### Complexity

```text
Time  : O(h)
Space : O(1)
```

Min/max in a BST are found by following one side of the tree.

---

# 7. BST Deletion — Concept

**Do not memorize the code yet.**

Understand the **3 cases**.

Suppose we want to delete `X`.

First use the BST property to find `X`.

```text
X < root->val
    ↓
LEFT

X > root->val
    ↓
RIGHT
```

Once found, there are three possibilities.

---

## Case 1 — Leaf Node

```text
       20
      /
    10
```

Delete `10`.

It has no children.

Simply remove it:

```text
       20
```

Conceptually:

```cpp
return nullptr;
```

---

# Case 2 — One Child

Example:

```text
       20
      /
    10
      \
       15
```

Delete `10`.

We cannot simply remove `10`, because `15` would become disconnected.

Instead:

```text
       20
      /
     15
```

The parent's pointer now points directly to the child.

Conceptually:

```cpp
if(root->left == nullptr)
    return root->right;

if(root->right == nullptr)
    return root->left;
```

---

# Case 3 — Two Children ⭐

This is the important one.

```text
          50
        /    \
      30      70
             /  \
            60   80
```

Delete `70`.

It has:

```text
Left child  = 60
Right child = 80
```

We cannot simply return one child because we must preserve the BST ordering.

Use either:

```text
Inorder Successor
```

or

```text
Inorder Predecessor
```

### Using Successor

Find the **smallest value in the right subtree**.

Right subtree:

```text
      80
     /
    60
```

Smallest = `60`.

Replace `70` with `60`.

```text
          50
        /    \
      30      60
                \
                 80
```

Then delete the original `60`.

The successor is the minimum node in the right subtree.

---

# BST Delete Mental Model

```text
Find node
    ↓
 ┌──┴─────────────┐
 ↓                ↓
0 children      1 child
 ↓                ↓
return null     return child
        \
         \
       2 children
            ↓
 Find inorder successor
            ↓
 Replace value
            ↓
 Delete successor
```

---

# Complexity of Deletion

```text
Time : O(h)
Space: O(h) recursive
```

Balanced BST:

```text
O(log n)
```

Worst-case skewed BST:

```text
O(n)
```

The two-child case may require another walk to find the successor, but that is still bounded by the tree height, so the overall operation remains `O(h)`.

---

# 🔥 Final Revision Table

| Problem / Operation | Main Idea              | Time | Extra Space |
| ------------------- | ---------------------- | ---: | ----------: |
| Height              | `1 + max(left,right)`  | O(n) |        O(h) |
| Count Nodes         | `1 + left + right`     | O(n) |        O(h) |
| Search BST          | Compare → left/right   | O(h) |       O(h)* |
| Insert BST          | Search until `nullptr` | O(h) |       O(h)* |
| Min BST             | Keep going left        | O(h) |        O(1) |
| Max BST             | Keep going right       | O(h) |        O(1) |
| Delete BST          | 3 cases                | O(h) |       O(h)* |

`h` = tree height.

For a balanced BST:

```text
h = O(log n)
```

For a skewed BST:

```text
h = O(n)
```

So:

```text
Balanced BST → O(log n)

Skewed BST   → O(n)
```

---

# 🧠 What You Must Remember

### Normal Binary Tree

```text
Search
↓
May need BOTH sides
```

### BST

```text
Compare
↓
ONE direction
↓
Left OR Right
```

### Minimum

```text
Keep going LEFT
```

### Maximum

```text
Keep going RIGHT
```

### Insert

```text
Find nullptr
↓
Create node
```

### Delete

```text
0 children → nullptr

1 child → return child

2 children → successor/predecessor
```

---

# Revision Order

Do these **in exactly this order**:

```text
1. Height
   ↓
2. Count Nodes
   ↓
3. Search BST
   ↓
4. Insert BST
   ↓
5. Min BST
   ↓
6. Max BST
   ↓
7. Delete BST concept
```

The first two refresh your basic recursion. Search → Insert then reinforces the BST decision rule. Min/Max are trivial once the BST property is fresh, and deletion comes last because it combines everything.
