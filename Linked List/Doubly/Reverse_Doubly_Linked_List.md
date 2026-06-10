# Reverse a Doubly Linked List

## Problem

Given the head of a doubly linked list, reverse the list and return the new head.

---

# Example

Input

```
1 <-> 2 <-> 3 <-> 4
```

Output

```
4 <-> 3 <-> 2 <-> 1
```

---

# Core Insight

This solution follows the same intuition as reversing a Singly Linked List.

Maintain:

* current node
* previous reversed node
* next node

and rebuild the links while updating both `next` and `prev`.

---

# Algorithm

```
current = head
last = nullptr

while(current)

    save next node

    current->prev = next node

    current->next = last

    last = current

    current = next node

return last
```

---

# Dry Run

Initial

```
1 <-> 2 <-> 3
```

Iteration 1

```
last = 1

1
```

Iteration 2

```
last = 2

2 <-> 1
```

Iteration 3

```
last = 3

3 <-> 2 <-> 1
```

Return

```
3
```

which becomes the new head.

---

# Complexity

```
Time  : O(n)

Space : O(1)
```

---

# Common Mistakes

### Returning current pointer

Wrong:

```cpp
return temp;
```

After traversal, `temp` becomes `nullptr`.

Return

```cpp
return last;
```

instead.

---

### Forgetting to update prev

Updating only `next` will break backward traversal.

Always maintain both pointers.

---

# Final Code

```cpp
class Solution {
public:

    Node* reverse(Node* head)
    {
        if(head == nullptr)
            return nullptr;

        if(head->next == nullptr)
            return head;

        Node* temp = head;
        Node* last = nullptr;

        while(temp)
        {
            Node* nextNode = temp->next;

            temp->prev = nextNode;
            temp->next = last;

            last = temp;
            temp = nextNode;
        }

        return last;
    }
};
```

---

# Pattern

```
Reverse Singly Linked List

+

Maintain prev pointer

=

Reverse Doubly Linked List
```

---

# Interview One-Liner

> Traverse once, rebuild the `next` links exactly like reversing a singly linked list while simultaneously updating the `prev` pointer, and return the last processed node as the new head.

---

# Notebook One-Liner

> Store the next node, update both `prev` and `next`, move forward, and return the last visited node as the new head.
