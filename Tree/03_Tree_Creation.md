# Tree Creation

Before solving tree problems, we should know how a binary tree is created. In interviews and coding platforms, trees are commonly created in two ways.

---

# 1. Manual Creation (Using Pointers)

This is the easiest way to understand how nodes are connected.

## Tree

```text
        1
      /   \
     2     3
          / \
         4   5
```

## Code

```cpp
#include <iostream>
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

int main() {

    Node* root = new Node(1);

    root->left = new Node(2);
    root->right = new Node(3);

    root->right->left = new Node(4);
    root->right->right = new Node(5);

    return 0;
}
```

### Explanation

* Create the root node.
* Connect the left child.
* Connect the right child.
* Continue connecting child nodes using pointers.

This method is mainly used to understand tree pointers and for small examples.

---

# 2. Recursive Creation (Preorder + NULL)

Here, the tree is represented using **Preorder Traversal**, where **-1** indicates that a child does not exist.

Input:

```text
1 2 -1 -1 3 4 -1 -1 5 -1 -1
```

Tree:

```text
        1
      /   \
     2     3
          / \
         4   5
```

## Code

```cpp
#include <iostream>
#include <vector>
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

int idx = -1;

Node* buildTree(vector<int>& preorder) {

    idx++;

    if (preorder[idx] == -1)
        return nullptr;

    Node* root = new Node(preorder[idx]);

    root->left = buildTree(preorder);
    root->right = buildTree(preorder);

    return root;
}

int main() {

    vector<int> preorder = {
        1,2,-1,-1,3,4,-1,-1,5,-1,-1
    };

    Node* root = buildTree(preorder);

    cout << root->data << endl;
}
```

### How It Works

The preorder array follows:

```
Root → Left → Right
```

Steps:

1. Read the current value.
2. If value is **-1**, return `NULL`.
3. Otherwise, create a node.
4. Recursively build the left subtree.
5. Recursively build the right subtree.
6. Return the completed subtree.

The recursion automatically attaches every node to its correct parent.

---

# Which Method is Used?

| Method                      | Usage                                                      |
| --------------------------- | ---------------------------------------------------------- |
| Manual Pointer Creation     | Learning tree structure and small examples                 |
| Recursive (Preorder + NULL) | Teaching recursion and tree construction                   |
| Level Order (Queue)         | Some coding questions (covered later)                      |
| LeetCode                    | Tree is already created; only `TreeNode* root` is provided |

---

# Key Takeaways

* Manual creation helps understand pointers.
* Recursive creation builds the entire tree from preorder input.
* Most coding platforms (LeetCode) do **not** ask you to build the tree.
* Understanding these creation methods makes traversals and recursive tree problems much easier.
