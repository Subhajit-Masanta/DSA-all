# 102. Binary Tree Level Order Traversal

| Topic | Difficulty | Pattern |
|-------|------------|---------|
| Tree | Medium | BFS / Queue |

---

## 📌 Problem Description

Given the root of a binary tree, return the **level order traversal** of its nodes' values.

Traverse the tree **level by level**, from **left to right**.

---

## 🧠 Intuition

Unlike Preorder, Inorder, and Postorder traversals which use **DFS (Depth First Search)**, this problem requires visiting **all nodes of one level before moving to the next**.

This is exactly what **BFS (Breadth First Search)** does.

A **Queue** naturally processes nodes in level order (FIFO), making it the ideal data structure.

---

## 🎯 Pattern Recognition

Keywords:

- BFS
- Queue
- Level Order Traversal
- Level-by-Level Processing

---

## ⚙️ Approach

- Push the root into a queue.
- While the queue is not empty:
  - Find the number of nodes in the current level using `queue.size()`.
  - Process exactly those nodes.
  - Store their values in a temporary vector.
  - Push their left and right children into the queue.
  - Add the current level to the answer.

---

## 🧪 Dry Run

Tree:

```text
        3
      /   \
     9     20
          /  \
         15   7
```

Queue:

```text
[3]
```

Level 1:

```text
Current = [3]
Queue = [9,20]
```

Level 2:

```text
Current = [9,20]
Queue = [15,7]
```

Level 3:

```text
Current = [15,7]
Queue = []
```

Output:

```text
[
 [3],
 [9,20],
 [15,7]
]
```

---

## ⚠️ Common Mistakes

### 1. Using DFS Instead of BFS

This problem requires **level-by-level traversal**, so use a **Queue**, not recursion.

---

### 2. Forgetting `queue.size()`

Without storing the current queue size, you cannot determine where one level ends and the next begins.

---

### 3. Adding the Current Level Too Early

Push the current level into the answer **after** processing all nodes of that level.

---

## 🎯 Key Takeaways

- Uses **BFS (Breadth First Search)**.
- Queue stores **TreeNode***, not node values.
- `queue.size()` determines the number of nodes in the current level.
- Every node is visited exactly once.

---

## 💻 Implementation (C++)

```cpp
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {

        vector<vector<int>> ans;

        if(root == nullptr)
            return ans;

        queue<TreeNode*> q;
        q.push(root);

        while(!q.empty()){

            int size = q.size();
            vector<int> current;

            while(size--){

                TreeNode* node = q.front();
                q.pop();

                current.push_back(node->val);

                if(node->left)
                    q.push(node->left);

                if(node->right)
                    q.push(node->right);
            }

            ans.push_back(current);
        }

        return ans;
    }
};
```

---

## ⏱️ Complexity Analysis

| Complexity | Value |
|------------|-------|
| Time | **O(n)** |
| Space | **O(n)** |

Where:

- **n** = Number of Nodes
- Queue stores at most one level of the tree.
