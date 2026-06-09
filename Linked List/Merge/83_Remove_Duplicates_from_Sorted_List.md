# 83. Remove Duplicates from Sorted List

## Problem

Given the head of a **sorted** linked list, remove duplicate nodes so that every value appears only once.

Return the modified linked list.

---

# Example 1

Input

```text
1 -> 1 -> 2
```

Output

```text
1 -> 2
```

---

# Example 2

Input

```text
1 -> 1 -> 2 -> 3 -> 3
```

Output

```text
1 -> 2 -> 3
```

---

# Core Insight

The list is already sorted.

Therefore,

```text
Duplicate values are always adjacent.
```

We only need to compare

```text
Current Node

and

Next Node
```

If both values are equal,

delete the next node.

Otherwise,

move to the next node.

---

# Pattern

```text
Single Pointer Traversal

+

Delete Next Node

+

Stay On Current Node
```

---

# Algorithm

### Step 1

Create one pointer

```text
temp = head
```

---

### Step 2

Traverse while

```text
temp

and

temp->next

exist
```

---

### Step 3

Compare

```text
temp->val

and

temp->next->val
```

---

### Step 4

If values are equal

```text
Save duplicate node

↓

Reconnect

↓

Delete duplicate

↓

Stay on same node
```

---

### Step 5

Otherwise

```text
Move temp forward
```

Repeat until the list ends.

---

# Dry Run

Input

```text
1 -> 1 -> 1 -> 2 -> 3 -> 3
```

---

## Initial

```text
temp

↓

1 -> 1 -> 1 -> 2 -> 3 -> 3
```

Compare

```text
1

1
```

Duplicate found.

---

## Delete

Save

```cpp
del = temp->next;
```

Reconnect

```cpp
temp->next = temp->next->next;
```

Delete

```cpp
delete del;
```

List becomes

```text
1 -> 1 -> 2 -> 3 -> 3
```

Notice

```text
temp
```

does NOT move.

---

## Compare Again

```text
1

1
```

Duplicate again.

Delete.

List becomes

```text
1 -> 2 -> 3 -> 3
```

Still

```text
temp = first node
```

---

## Compare

```text
1

2
```

Different.

Now move

```cpp
temp = temp->next;
```

---

## Continue

Compare

```text
2

3
```

Different.

Move.

Compare

```text
3

3
```

Duplicate.

Delete.

Final

```text
1 -> 2 -> 3
```

---

# Why Temp Does Not Move After Deletion?

Example

```text
1 -> 1 -> 1 -> 2
```

If temp moves immediately,

```text
1 -> 1 -> 2
     ↑
```

one duplicate remains.

Therefore,

after deleting,

we stay on the same node and compare again.

---

# Pointer Visualization

Before

```text
temp

↓

1 -> 1 -> 2 -> 3
```

Save

```cpp
ListNode* del = temp->next;
```

Reconnect

```cpp
temp->next = temp->next->next;
```

Now

```text
1 -> 2 -> 3

1 (del)
```

Delete

```cpp
delete del;
```

Final

```text
1 -> 2 -> 3
```

---

# Complexity

```text
Time  : O(n)

Space : O(1)
```

---

# Common Mistakes

### Mistake 1

Move temp after deletion.

Wrong.

This skips checking consecutive duplicates.

---

### Mistake 2

Delete before reconnecting.

Wrong.

Always

```text
Save

↓

Reconnect

↓

Delete
```

---

### Mistake 3

Using two nested loops.

Not required because the list is sorted.

One traversal is enough.

---

# Pattern Recognition

This belongs to

```text
Linked List Traversal
```

Pattern

```text
Compare Current

↓

Delete Next

↓

Stay

↓

Move only when values differ
```

---

# Interview One-Liner

> Since the list is sorted, duplicates are adjacent. Compare the current node with the next node, delete the duplicate node if values match, otherwise move forward.

---

# Final Code

```cpp
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {

        ListNode* temp = head;

        while(temp && temp->next)
        {
            if(temp->val == temp->next->val)
            {
                ListNode* del = temp->next;

                temp->next = temp->next->next;

                delete del;
            }
            else
            {
                temp = temp->next;
            }
        }

        return head;
    }
};
```

---

# Notebook One-Liner

> In a sorted linked list, duplicates are adjacent. Delete `temp->next` when values match and keep `temp` at the same position until the next value becomes unique.
