# Session 3 — BST + Recursion + Creation and Input

**Revision time:** 45–60 min

---

# 0. BST Structure

A BST follows:

```text
Left < Root < Right
```

Example:

```text
        50
       /  \
     30    70
    / \    / \
   20 40  60 80
```

---

# 1. Creating a BST from Values

If the input is:

```text
50 30 70 20 40 60 80
```

We insert the values **one by one**.

```text
50
```

becomes root.

Then:

```text
30 < 50 → left
70 > 50 → right
20 < 50 → left
20 < 30 → left
40 < 50 → left
40 > 30 → right
...
```

Final:

```text
        50
       /  \
     30    70
    / \    / \
   20 40  60 80
```

This is standard BST insertion: if the current node is `nullptr`, create the new node; otherwise move left or right according to the BST property.

---

# 2. Node Structure

For your own C++ programs:

```cpp
class Node {
public:
    int data;
    Node* left;
    Node* right;

    Node(int val) {
        data = val;
        left = nullptr;
        right = nullptr;
    }
};
```

Think:

```text
Node
│
├── data
├── left  → left child
└── right → right child
```

---

# 3. BST Insert Function

```cpp
Node* insert(Node* root, int val)
{
    if(root == nullptr)
        return new Node(val);

    if(val < root->data)
        root->left = insert(root->left, val);
    else
        root->right = insert(root->right, val);

    return root;
}
```

### The important pointer part

Suppose:

```text
        50
       /
     30
```

Insert `20`.

```cpp
root->left = insert(root->left, 20);
```

The recursion eventually reaches:

```text
30->left == nullptr
```

Then:

```cpp
return new Node(20);
```

So:

```text
30->left = [new node 20]
```

Then the recursion returns `30`, then `50`.

---

# 4. Creating the Whole BST from Input

Suppose input is:

```text
7
50 30 70 20 40 60 80
```

Where:

```text
7 = number of nodes
```

We can do:

```cpp
int n;
cin >> n;

Node* root = nullptr;

for(int i = 0; i < n; i++)
{
    int x;
    cin >> x;

    root = insert(root, x);
}
```

After the loop:

```text
        50
       /  \
     30    70
    / \    / \
   20 40  60 80
```

---

# 5. Complete Basic BST Program

This is the version you should remember for **OA-style standalone C++ questions**.

```cpp
#include <bits/stdc++.h>
using namespace std;

class Node {
public:
    int data;
    Node* left;
    Node* right;

    Node(int val) {
        data = val;
        left = nullptr;
        right = nullptr;
    }
};

Node* insert(Node* root, int val)
{
    if(root == nullptr)
        return new Node(val);

    if(val < root->data)
        root->left = insert(root->left, val);
    else
        root->right = insert(root->right, val);

    return root;
}

int main()
{
    int n;
    cin >> n;

    Node* root = nullptr;

    for(int i = 0; i < n; i++)
    {
        int x;
        cin >> x;

        root = insert(root, x);
    }

    return 0;
}
```

---

# 6. Example Input

```text
7
50 30 70 20 40 60 80
```

Creates:

```text
        50
       /  \
     30    70
    / \    / \
   20 40  60 80
```

---

# 7. Add Inorder Traversal to Check the BST

Because inorder traversal of a BST gives values in sorted order:

```cpp
void inorder(Node* root)
{
    if(root == nullptr)
        return;

    inorder(root->left);

    cout << root->data << " ";

    inorder(root->right);
}
```

Then:

```cpp
int main()
{
    int n;
    cin >> n;

    Node* root = nullptr;

    for(int i = 0; i < n; i++)
    {
        int x;
        cin >> x;
        root = insert(root, x);
    }

    inorder(root);
}
```

Input:

```text
7
50 30 70 20 40 60 80
```

Output:

```text
20 30 40 50 60 70 80
```

That is a quick way to verify that your constructed tree obeys the BST ordering.

---

# 8. If Input Is Already Given as a Sorted Array

This is different from inserting values one by one.

For:

```text
-10 -3 0 5 9
```

If the question asks for a **height-balanced BST**, use the middle element recursively.

```cpp
TreeNode* solve(vector<int>& nums, int low, int high)
{
    if(low > high)
        return nullptr;

    int mid = low + (high - low) / 2;

    TreeNode* node = new TreeNode(nums[mid]);

    node->left = solve(nums, low, mid - 1);
    node->right = solve(nums, mid + 1, high);

    return node;
}
```

This is your **LeetCode 108** pattern.

---

# 9. Two Ways You Should Distinguish

## Method 1 — Values arrive one by one

Example:

```text
50 30 70 20 40 60 80
```

Use:

```text
Insert each value
        ↓
BST insertion
```

Tree shape depends on insertion order.

---

## Method 2 — Sorted array + balanced BST requested

Example:

```text
[-10,-3,0,5,9]
```

Use:

```text
Choose middle
      ↓
Left half → left subtree
Right half → right subtree
```

This produces a height-balanced BST.

---

# 10. Important Difference

| Situation                      | What to do                                  |
| ------------------------------ | ------------------------------------------- |
| Values given one by one        | Insert sequentially                         |
| Need BST from arbitrary values | Sequential insertion                        |
| Sorted array + balanced BST    | Choose middle recursively                   |
| LeetCode BST problem           | Usually tree is already given               |
| OA/college standalone program  | You may need to construct the tree yourself |

---

# 11. Complexity

### Creating BST using insertion

For `n` values:

```text
Average / balanced:

O(n log n)
```

because each insertion takes approximately `O(log n)`.

Worst case:

```text
O(n²)
```

if insertion creates a completely skewed tree:

```text
1
 \
  2
   \
    3
     \
      4
```

BST insertion itself takes `O(h)`, where `h` is the tree height.

### Sorted Array → Balanced BST

```text
Time  : O(n)
Space : O(log n)
```

Every element becomes one node, and recursion depth is proportional to the balanced tree height.

---

# 12. ⭐ What to Remember for OA

If they give:

```text
n
values...
```

and ask:

> Build a BST.

Immediately think:

```cpp
Node* root = nullptr;

for(...)
    root = insert(root, x);
```

And your insertion template:

```cpp
Node* insert(Node* root, int val)
{
    if(root == nullptr)
        return new Node(val);

    if(val < root->data)
        root->left = insert(root->left, val);
    else
        root->right = insert(root->right, val);

    return root;
}
```

---

# 13. One Important Warning

Do **not** confuse BST insertion with ordinary Binary Tree insertion.

### BST

```text
Compare value
     ↓
Left / Right
     ↓
nullptr
     ↓
Insert
```

### Ordinary Binary Tree

There is no:

```text
Left < Root < Right
```

rule.

If you want to insert while maintaining a complete/level-order shape, you typically use level-order traversal to find the first available position.

---

# Final Session 3 Checklist

```text
□ BST Node structure

□ Create BST from input
      ↓
   insert()

□ Height
□ Count Nodes

□ Search BST
□ Insert BST

□ Minimum BST
□ Maximum BST

□ Delete BST
   ├── 0 children
   ├── 1 child
   └── 2 children

□ Inorder → sorted BST values
```

### Core templates to memorize

```text
BST Search
    ↓
Compare → Left / Right

BST Insert
    ↓
Compare → Left / Right → nullptr → New Node

BST Min
    ↓
Keep going LEFT

BST Max
    ↓
Keep going RIGHT

BST Delete
    ↓
0 child → nullptr
1 child → child
2 children → successor/predecessor
```
