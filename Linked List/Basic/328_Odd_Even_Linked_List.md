# 328. Odd Even Linked List

## Problem

Given the head of a singly linked list, group all **odd indexed nodes** together followed by all **even indexed nodes**.

**Important**

* Odd/Even refers to **position**, **NOT value**.
* Relative order inside odd and even groups must remain the same.

---

## Example

Input

```text
1 -> 2 -> 3 -> 4 -> 5
```

Output

```text
1 -> 3 -> 5 -> 2 -> 4
```

---

## Core Insight

Instead of creating a new list,

maintain **two separate chains**.

```text
Odd Chain

1 -> 3 -> 5

Even Chain

2 -> 4
```

After traversal,

connect

```text
Odd Tail

↓

Even Head
```

Final Result

```text
1 -> 3 -> 5 -> 2 -> 4
```

---

# Why Three Pointers?

```cpp
ListNode* odd = head;
ListNode* even = head->next;
ListNode* evenHead = even;
```

---

## odd

Points to the last node of the odd chain.

Initially

```text
odd

1 -> 2 -> 3 -> 4 -> 5
```

---

## even

Points to the last node of the even chain.

Initially

```text
even

2 -> 3 -> 4 -> 5
```

---

## evenHead (Most Important)

Stores the **starting node of the even list**.

```text
evenHead

2 -> 4
```

Why?

Because during traversal,

```cpp
even = even->next;
```

keeps moving.

Without saving the beginning,

we lose access to the even list.

Finally,

```cpp
odd->next = evenHead;
```

joins both lists.

---

# While Condition

```cpp
while (even && even->next)
```

Equivalent to

```cpp
while (even != nullptr && even->next != nullptr)
```

---

## Why this condition?

Inside the loop we use

```cpp
odd->next = even->next;
```

So we must guarantee

```text
even exists

AND

even->next exists
```

Otherwise,

accessing

```cpp
even->next
```

causes a segmentation fault.

---

# Dry Run

## Initial

```text
1 -> 2 -> 3 -> 4 -> 5

odd = 1
even = 2
evenHead = 2
```

---

## Iteration 1

### Step 1

```cpp
odd->next = even->next;
```

```text
1 -------> 3
```

Odd chain

```text
1 -> 3
```

---

### Step 2

```cpp
odd = odd->next;
```

```text
odd

3
```

---

### Step 3

```cpp
even->next = odd->next;
```

```text
2 -------> 4
```

Even chain

```text
2 -> 4
```

---

### Step 4

```cpp
even = even->next;
```

```text
even

4
```

---

## Iteration 2

Current

```text
Odd

1 -> 3 -> 4 -> 5

Even

2 -> 4 -> 5
```

---

### Step 1

```cpp
odd->next = even->next;
```

```text
3 -------> 5
```

Odd chain

```text
1 -> 3 -> 5
```

---

### Step 2

```cpp
odd = odd->next;
```

```text
odd

5
```

---

### Step 3

```cpp
even->next = odd->next;
```

```text
4 -------> NULL
```

Even chain

```text
2 -> 4
```

---

### Step 4

```cpp
even = even->next;
```

```text
even = NULL
```

Loop stops.

---

# Final Step

Join both lists.

```cpp
odd->next = evenHead;
```

Result

```text
1 -> 3 -> 5 -> 2 -> 4
```

---

# Algorithm

1. Create three pointers

```text
odd
even
evenHead
```

2. Rearrange odd links.

3. Rearrange even links.

4. Move odd and even pointers.

5. Repeat until even or even->next becomes NULL.

6. Connect odd list with evenHead.

---

# Code

```cpp
class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {

        if (!head || !head->next)
            return head;

        ListNode* odd = head;
        ListNode* even = head->next;
        ListNode* evenHead = even;

        while (even && even->next)
        {
            odd->next = even->next;
            odd = odd->next;

            even->next = odd->next;
            even = even->next;
        }

        odd->next = evenHead;

        return head;
    }
};
```

---

# Complexity

```text
Time  : O(n)

Space : O(1)
```

Only pointer manipulation.

No extra linked list is created.

---

# Common Mistakes

### Forgetting evenHead

```cpp
odd->next = even;
```

Wrong.

`even` moves during traversal.

Need

```cpp
odd->next = evenHead;
```

---

### Wrong While Condition

```cpp
while(even)
```

Wrong.

Inside loop,

```cpp
even->next
```

is accessed.

Need

```cpp
while(even && even->next)
```

---

### Confusing Odd Values and Odd Indices

Wrong

```text
1 3 5
```

because values are odd.

Correct

```text
Position

1st
3rd
5th
```

---

# Pattern Learned

## Two Independent Chains

```text
Odd Chain

1 -> 3 -> 5

Even Chain

2 -> 4

↓

Join

1 -> 3 -> 5 -> 2 -> 4
```

---

# Interview One-Liner

> Maintain two separate linked lists (odd and even positions) using pointer manipulation, preserve the head of the even list (`evenHead`), and finally connect the odd list to the even list in **O(n)** time and **O(1)** extra space.

---

# Notebook One-Liner

> **Whenever a linked list problem asks for grouping while preserving order, think of building multiple chains using pointers and joining them at the end instead of creating new lists.**
