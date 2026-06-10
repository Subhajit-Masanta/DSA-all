# Doubly_Linked_List_Basics.md

# Doubly Linked List - Basics

## What is a Doubly Linked List?

A Doubly Linked List (DLL) is a linear data structure where every node stores:

* Previous Node Address (`prev`)
* Data
* Next Node Address (`next`)

```text
+--------+--------+--------+
|  prev  |  data  |  next  |
+--------+--------+--------+
```

Unlike a Singly Linked List, a Doubly Linked List allows traversal in both directions.

---

# Node Structure

```cpp
class Node
{
public:
    int data;
    Node* prev;
    Node* next;

    Node(int x)
    {
        data = x;
        prev = nullptr;
        next = nullptr;
    }
};
```

---

# Memory Representation

```text
head
 ↓

NULL <- 10 <-> 20 <-> 30 <-> 40 -> NULL
```

Every node knows both its previous and next node.

---

# Forward Traversal

## Algorithm

```
temp = head

while(temp != nullptr)
{
    visit(temp)
    temp = temp->next
}
```

## Code

```cpp
void forwardTraversal()
{
    if(head == nullptr)
    {
        cout<<"List is empty"<<endl;
        return;
    }

    Node* temp = head;

    while(temp)
    {
        cout<<temp->data<<" ";
        temp = temp->next;
    }

    cout<<endl;
}
```

### Complexity

```
Time  : O(n)
Space : O(1)
```

---

# Backward Traversal

First reach the last node and then move using `prev`.

## Algorithm

```
Go to last node

while(temp->next)
    temp=temp->next

while(temp)
{
    visit(temp)
    temp=temp->prev
}
```

## Code

```cpp
void backwardTraversal()
{
    if(head == nullptr)
    {
        cout<<"List is empty"<<endl;
        return;
    }

    Node* temp = head;

    while(temp->next)
    {
        temp = temp->next;
    }

    while(temp)
    {
        cout<<temp->data<<" ";
        temp = temp->prev;
    }

    cout<<endl;
}
```

### Complexity

```
Time  : O(n)
Space : O(1)
```

---

# Insert at Front

## Pointer Updates

```
newNode->next = head

head->prev = newNode

head = newNode
```

## Code

```cpp
void insertFront(int x)
{
    Node* newNode = new Node(x);

    if(head == nullptr)
    {
        head = newNode;
        return;
    }

    newNode->next = head;
    head->prev = newNode;
    head = newNode;
}
```

### Complexity

```
Time  : O(1)
Space : O(1)
```

---

# Insert at End

## Pointer Updates

```
last->next = newNode

newNode->prev = last
```

## Code

```cpp
void insertEnd(int x)
{
    Node* newNode = new Node(x);

    if(head == nullptr)
    {
        head = newNode;
        return;
    }

    Node* temp = head;

    while(temp->next)
    {
        temp = temp->next;
    }

    temp->next = newNode;
    newNode->prev = temp;
}
```

### Complexity

```
Time  : O(n)
Space : O(1)
```

---

# Delete Front

## Pointer Updates

```
temp = head

head = head->next

head->prev = nullptr

delete temp
```

## Code

```cpp
void deleteFront()
{
    if(head == nullptr)
    {
        cout<<"List is empty"<<endl;
        return;
    }

    Node* temp = head;

    head = head->next;

    if(head)
    {
        head->prev = nullptr;
    }

    delete temp;
}
```

### Complexity

```
Time  : O(1)
Space : O(1)
```

---

# Delete End

## Pointer Updates

```
last->prev->next = nullptr

delete last
```

## Code

```cpp
void deleteEnd()
{
    if(head == nullptr)
    {
        cout<<"List is empty"<<endl;
        return;
    }

    if(head->next == nullptr)
    {
        delete head;
        head = nullptr;
        return;
    }

    Node* temp = head;

    while(temp->next)
    {
        temp = temp->next;
    }

    temp->prev->next = nullptr;

    delete temp;
}
```

### Complexity

```
Time  : O(n)
Space : O(1)
```

---

# Singly vs Doubly Linked List

| Operation          | Singly             | Doubly                            |
| ------------------ | ------------------ | --------------------------------- |
| Forward Traversal  | ✅                  | ✅                                 |
| Backward Traversal | ❌                  | ✅                                 |
| Insert Front       | O(1)               | O(1)                              |
| Insert End         | O(n)               | O(n)                              |
| Delete Front       | O(1)               | O(1)                              |
| Delete End         | O(n)               | O(n)                              |
| Delete Given Node  | Need Previous Node | O(1) if node pointer is available |

---

# Interview Notes

Always remember:

```
Whenever next changes,

ask yourself,

Should prev also change?
```

---

# Notebook One-Liner

> A Doubly Linked List stores both previous and next pointers, allowing bidirectional traversal and O(1) insertion/deletion when the target node is already known.
