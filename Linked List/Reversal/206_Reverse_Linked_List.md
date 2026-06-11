# Reverse Linked List

| Topic       | Difficulty | Pattern                              |
| :---------- | :--------- | :----------------------------------- |
| Linked List | Easy       | Three Pointer / Pointer Manipulation |

---

# 📌 Problem Description

Given the head of a singly linked list, reverse the linked list and return the new head.

Example:

```text
Input:
1 -> 2 -> 3 -> 4 -> 5

Output:
5 -> 4 -> 3 -> 2 -> 1
```

---

# 🧠 Core Insight

A linked list is connected using **next pointers**.

Original:

```text
1 -> 2 -> 3 -> 4 -> 5
```

After reversing:

```text
1 <- 2 <- 3 <- 4 <- 5
```

The challenge is that changing a pointer may disconnect the remaining list.

Before reversing a link, always **save the next node**.

---

# ⚙️ Algorithm

Maintain three pointers:

```text
prev
curr
next
```

### Step 1

Save the next node.

```cpp
next = curr->next;
```

### Step 2

Reverse the current link.

```cpp
curr->next = prev;
```

### Step 3

Move `prev` forward.

```cpp
prev = curr;
```

### Step 4

Move `curr` forward.

```cpp
curr = next;
```

Repeat until

```cpp
curr == nullptr
```

Return

```cpp
prev
```

because it becomes the new head.

---

# 🧪 Example / Dry Run

Input

```text
1 -> 2 -> 3 -> 4 -> 5
```

---

### Initial State

```text
prev = NULL

curr = 1

NULL    1 -> 2 -> 3 -> 4 -> 5
 ^       ^
prev    curr
```

---

### Iteration 1

Save

```text
next = 2
```

Reverse

```text
NULL <- 1     2 -> 3 -> 4 -> 5
```

Move pointers

```text
prev = 1

curr = 2
```

---

### Iteration 2

Save

```text
next = 3
```

Reverse

```text
NULL <- 1 <- 2     3 -> 4 -> 5
```

Move

```text
prev = 2

curr = 3
```

---

### Iteration 3

```text
NULL <- 1 <- 2 <- 3     4 -> 5
```

---

### Iteration 4

```text
NULL <- 1 <- 2 <- 3 <- 4     5
```

---

### Iteration 5

```text
NULL <- 1 <- 2 <- 3 <- 4 <- 5
```

Now

```text
curr = NULL

prev = 5
```

Return

```text
5
```

which is the new head.

---

# ⏱ Time Complexity

```text
O(n)
```

Only one traversal is performed.

---

# 📦 Space Complexity

```text
O(1)
```

Only three pointer variables are used.

---

# ⚠️ Common Mistakes

### Mistake 1

Not saving the next node.

Wrong

```cpp
curr->next = prev;
curr = curr->next;
```

The remaining list is lost.

Always save

```cpp
next = curr->next;
```

before reversing.

---

### Mistake 2

Returning

```cpp
return head;
```

Wrong.

`head` still points to the old first node.

---

### Mistake 3

Returning

```cpp
return curr;
```

Wrong.

At the end

```text
curr = NULL
```

---

### Correct

```cpp
return prev;
```

because `prev` points to the new head.

---

# 🔥 Key Insight

Always remember:

```text
Save Next

↓

Reverse Link

↓

Move Prev

↓

Move Curr
```

Never reverse a link without saving the next node.

---

# 🎯 Interview One-Liner

Reverse a linked list using the three-pointer technique (`prev`, `curr`, `next`) by reversing one link at a time while preserving access to the remaining nodes.

---

# 💻 Code (C++)

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {

        ListNode* prev = nullptr;
        ListNode* curr = head;

        while(curr)
        {
            ListNode* next = curr->next;

            curr->next = prev;

            prev = curr;

            curr = next;
        }

        return prev;
    }
};
```

---

# 📚 Pattern Recognition

If the problem says:

```text
Reverse Linked List

Reverse Sublist

Reverse Every K Nodes

Reverse Second Half
```

Think:

```text
Three Pointer Pattern

prev

↓

curr

↓

next

↓

Reverse Link

↓

Move Forward
```

This pattern is reused in:

```text
206 Reverse Linked List

92 Reverse Linked List II

25 Reverse Nodes in K Group

234 Palindrome Linked List

143 Reorder List
```
