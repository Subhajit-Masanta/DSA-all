# Node Object vs Node Pointer (`Node` vs `Node*`)

## Core Idea

There are two different things:

1. **Node** → Actual object
2. **Node*** → Pointer that stores the address of a Node

Never confuse these two.

---

# 1. Creating a Node Object

```cpp
Node newNode(10);
```

or

```cpp
Node newNode;
```

## What happens?

A real Node object is created on the **stack**.

Memory:

```text
Stack

newNode

+----------------+
| data = 10      |
| next = NULL    |
+----------------+
```

`newNode` IS the actual object.

Access members:

```cpp
newNode.data
newNode.next
```

Use `.` because it is an object.

---

# 2. Creating a Node Pointer

```cpp
Node* newNode = new Node(10);
```

This statement has two parts.

## Step 1

```cpp
new Node(10)
```

Creates a Node on the **heap**.

Example:

```text
Heap

Address 5000

+----------------+
| data = 10      |
| next = NULL    |
+----------------+
```

---

## Step 2

```cpp
Node* newNode
```

Creates a pointer.

The pointer stores:

```text
5000
```

Memory:

```text
Stack

newNode

+------+
|5000  |
+------+

      |

      v

Heap

Address 5000

+----------------+
| data = 10      |
| next = NULL    |
+----------------+
```

The pointer is **not the Node**.

It only knows where the Node is stored.

---

# 3. Why Not This?

```cpp
Node newNode = new Node(10);
```

Compiler error.

Why?

Left side:

```cpp
Node newNode;
```

expects an **object**.

Right side:

```cpp
new Node(10)
```

returns a **pointer (Node*)**.

So compiler sees:

```text
Object = Pointer
```

Types do not match.

---

# 4. Type Matching

Correct:

```cpp
Node obj;
```

```text
Object = Object
```

Correct:

```cpp
Node* ptr = new Node(10);
```

```text
Pointer = Pointer
```

Wrong:

```cpp
Node obj = new Node(10);
```

```text
Object = Pointer
```

Compilation error.

---

# 5. Why Linked Lists Use `Node*`

Suppose:

```cpp
void insert()
{
    Node newNode(10);
}
```

`newNode` is created on the stack.

After function ends:

```text
Stack Memory

Function Ends

↓

newNode is automatically destroyed.
```

The node disappears.

---

Now:

```cpp
void insert()
{
    Node* newNode = new Node(10);
}
```

Memory:

```text
Stack

newNode -----> 5000

Heap

Address 5000

Node
```

When function ends,

only the pointer variable is destroyed.

The Node on the heap still exists.

This is exactly what Linked Lists need.

---

# 6. Object vs Pointer

## Object

```cpp
Node node(10);
```

Access:

```cpp
node.data
node.next
```

Uses `.`

---

## Pointer

```cpp
Node* node = new Node(10);
```

Access:

```cpp
node->data
node->next
```

Uses `->`

Equivalent to:

```cpp
(*node).data
(*node).next
```

---

# 7. Memory Comparison

## Object

```cpp
Node node(10);
```

```text
Stack

+----------------+
| data = 10      |
| next = NULL    |
+----------------+
```

---

## Pointer

```cpp
Node* node = new Node(10);
```

```text
Stack

+--------+
| 5000   |
+--------+

      |

      v

Heap

+----------------+
| data = 10      |
| next = NULL    |
+----------------+
```

---

# 8. Why Do We Prefer `Node*` in DSA?

* Nodes must remain alive after function calls.
* We connect nodes using addresses.
* LeetCode and interviews always use pointers.

Typical interview code:

```cpp
ListNode* head;
ListNode* curr;
ListNode* prev;
ListNode* temp;
```

Very rarely:

```cpp
ListNode node;
```

---

# Summary Table

| Code                        | Meaning                                     |
| --------------------------- | ------------------------------------------- |
| `Node node;`                | Create a Node object                        |
| `Node node(10);`            | Create & initialize a Node object           |
| `Node* ptr;`                | Create a pointer to a Node                  |
| `Node* ptr = new Node(10);` | Create a Node on heap and store its address |

---

# Golden Rules

### Rule 1

```text
Node
```

Actual object.

---

### Rule 2

```text
Node*
```

Pointer that stores the address of a Node.

---

### Rule 3

```cpp
new Node(...)
```

Always returns:

```text
Node*
```

never

```text
Node
```

---

### Rule 4

```cpp
Node* ptr = new Node(10);
```

✔ Correct

```cpp
Node obj = new Node(10);
```

✘ Wrong

---

# Interview One-Liner

> `Node` represents an actual object.
>
> `Node*` represents the memory address of a Node.
>
> `new Node()` creates a Node on the heap and returns its address, so it must be stored in a `Node*`.
