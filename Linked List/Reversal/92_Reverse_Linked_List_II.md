# Reverse Linked List II

| Topic | Difficulty | Pattern |
| :----- | :--------- | :------ |
| Linked List | Medium | Three Pointer + Sublist Reconnection |

---

# 📌 Problem Description

Given the head of a linked list and two integers `left` and `right`, reverse the nodes from position `left` to position `right`.

Return the modified linked list.

Example:

```text
Input:
1 -> 2 -> 3 -> 4 -> 5

left = 2
right = 4

Output:
1 -> 4 -> 3 -> 2 -> 5
```

Only the nodes between positions `left` and `right` are reversed.

---

# 🧠 Core Insight

This is an extension of **206. Reverse Linked List**.

Instead of reversing the entire list:

```text
1 -> 2 -> 3 -> 4 -> 5

↓

5 -> 4 -> 3 -> 2 -> 1
```

we reverse only a sublist.

```text
1 -> [2 -> 3 -> 4] -> 5

↓

1 -> [4 -> 3 -> 2] -> 5
```

The reversal logic is exactly the same.

The only extra work is reconnecting the reversed section.

---

# ⚙️ Algorithm

### Step 1

Move to position `left`.

Store:

```text
prevLeft      = node before left

sublistHead   = left node
```

---

### Step 2

Reverse exactly

```text
right - left + 1
```

nodes using the normal

```text
prev

↓

curr

↓

next
```

pattern.

---

### Step 3

Reconnect both ends.

```cpp
prevLeft->next = prev;

sublistHead->next = curr;
```

---

### Step 4

If

```text
left == 1
```

there is no previous node.

Update

```cpp
head = prev;
```

instead.

---

### Step 5

Return head.

---

# 🧪 Example / Dry Run

Input

```text
1 -> 2 -> 3 -> 4 -> 5

left = 2

right = 4
```

---

### Locate Boundaries

```text
prevLeft = 1

sublistHead = 2

1 -> 2 -> 3 -> 4 -> 5
     ^
sublistHead
```

---

### Reverse Sublist

Reverse

```text
2 -> 3 -> 4
```

Result

```text
4 -> 3 -> 2
```

Pointers become

```text
prev = 4

curr = 5
```

---

### Reconnect

First connection

```text
1 -> 4
```

Second connection

```text
2 -> 5
```

Final list

```text
1 -> 4 -> 3 -> 2 -> 5
```

---

# 🧪 Edge Case

Input

```text
1 -> 2 -> 3 -> 4 -> 5

left = 1

right = 3
```

Reverse

```text
1 -> 2 -> 3

↓

3 -> 2 -> 1
```

Now

```text
prevLeft = NULL
```

So

```cpp
head = prev;
```

Final answer

```text
3 -> 2 -> 1 -> 4 -> 5
```

---

# ⏱ Time Complexity

```text
O(n)
```

Only one traversal of the linked list.

---

# 📦 Space Complexity

```text
O(1)
```

Only pointer variables are used.

---

# ⚠️ Common Mistakes

### Mistake 1

Forgetting to reconnect the first half.

Wrong

```cpp
sublistHead->next = curr;
```

only.

Need

```cpp
prevLeft->next = prev;
```

also.

---

### Mistake 2

Forgetting to reconnect the tail.

Wrong

```text
1 -> 4 -> 3 -> 2

5
```

Correct

```cpp
sublistHead->next = curr;
```

---

### Mistake 3

Ignoring

```text
left == 1
```

There is no previous node.

Update

```cpp
head = prev;
```

instead of

```cpp
prevLeft->next = prev;
```

---

### Mistake 4

Reversing the wrong number of nodes.

Reverse exactly

```text
right - left + 1
```

nodes.

---

# 🔥 Key Insight

Think of the problem as

```text
Find Left Boundary

↓

Reverse Sublist

↓

Reconnect Left Side

↓

Reconnect Right Side
```

instead of trying to manipulate the entire linked list.

---

# 🎯 Interview One-Liner

Reverse only the required sublist using the standard three-pointer reversal technique and reconnect the reversed section with the unreversed parts of the linked list.

---

# 💻 Code (C++)

```cpp
class Solution {
public:

    ListNode* reverseBetween(ListNode* head, int left, int right) {

        if(head == nullptr || left == right)
            return head;

        ListNode* prevLeft = nullptr;
        ListNode* sublistHead = head;

        for(int i = 1; i < left; i++)
        {
            prevLeft = sublistHead;
            sublistHead = sublistHead->next;
        }

        ListNode* prev = nullptr;
        ListNode* curr = sublistHead;
        ListNode* next = nullptr;

        for(int i = 0; i < right - left + 1; i++)
        {
            next = curr->next;

            curr->next = prev;

            prev = curr;

            curr = next;
        }

        if(prevLeft)
            prevLeft->next = prev;
        else
            head = prev;

        sublistHead->next = curr;

        return head;
    }
};
```

---

# 📚 Pattern Recognition

If the problem says

```text
Reverse Part of Linked List

Reverse Between Positions

Reverse Range

Reverse Sublist
```

Think

```text
Find Left Boundary

↓

Three Pointer Reverse

↓

Reconnect Both Ends
```

instead of

```text
Reverse Entire Linked List
```

This pattern is reused in:

```text
92 Reverse Linked List II

25 Reverse Nodes in K Group

143 Reorder List

Many Linked List Segment Problems
```
