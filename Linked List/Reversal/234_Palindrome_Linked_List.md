# Palindrome Linked List

| Topic       | Difficulty | Pattern                                   |
| :---------- | :--------- | :---------------------------------------- |
| Linked List | Easy       | Fast & Slow Pointer + Reverse Linked List |

---

# 📌 Problem Description

Given the head of a singly linked list, determine whether it is a palindrome.

Return:

```text
true  -> if the linked list is a palindrome

false -> otherwise
```

A palindrome reads the same forward and backward.

---

# 🧠 Core Insight

Instead of using extra space, we can compare the first half and the second half of the linked list.

The problem combines two previously learned patterns:

```text
Find Middle

↓

Reverse Second Half

↓

Compare Both Halves
```

---

# ⚙️ Algorithm

### Step 1

Find the middle using Fast & Slow pointers.

```cpp
slow = head;
fast = head;

while(fast && fast->next)
{
    slow = slow->next;
    fast = fast->next->next;
}
```

After the loop,

```text
slow
```

points to the middle node.

---

### Step 2

Reverse the second half starting from `slow`.

```cpp
ListNode* second = reverse(slow);
```

---

### Step 3

Compare the first half and the reversed second half.

```cpp
while(second)
{
    if(head->val != second->val)
        return false;

    head = head->next;
    second = second->next;
}
```

If every value matches,

```cpp
return true;
```

---

# 🧪 Example / Dry Run

Input

```text
1 -> 2 -> 2 -> 1
```

---

### Find Middle

```text
1 -> 2 -> 2 -> 1
          ^
        slow
```

---

### Reverse Second Half

Original

```text
2 -> 1
```

After reverse

```text
1 -> 2
```

Now

```text
First Half

1 -> 2

Second Half

1 -> 2
```

---

### Compare

```text
1 == 1 ✅

2 == 2 ✅
```

All nodes match.

Return

```text
true
```

---

# 🧪 Odd Length Example

Input

```text
1 -> 2 -> 3 -> 2 -> 1
```

Find middle

```text
1 -> 2 -> 3 -> 2 -> 1
          ^
         slow
```

Reverse from middle

```text
3 -> 2 -> 1

↓

1 -> 2 -> 3
```

Compare

```text
1 == 1

2 == 2

3 == 3
```

Return

```text
true
```

---

# ⏱ Time Complexity

```text
Find Middle     O(n)

Reverse Half    O(n)

Compare         O(n)

Total           O(n)
```

---

# 📦 Space Complexity

```text
O(1)
```

No extra data structure is used.

---

# ⚠️ Common Mistakes

### Mistake 1

Using

```cpp
while(head)
```

Wrong.

The reversed second half may become shorter.

Eventually,

```cpp
second == nullptr
```

and accessing

```cpp
second->val
```

causes a crash.

Always use

```cpp
while(second)
```

---

### Mistake 2

Calling

```cpp
reverse(slow);
```

without storing the returned head.

Correct

```cpp
ListNode* second = reverse(slow);
```

---

### Mistake 3

Comparing node addresses.

Wrong

```cpp
if(head == second)
```

Correct

```cpp
if(head->val != second->val)
```

---

### Mistake 4

Forgetting

```cpp
return prev;
```

inside the reverse function.

The reversed list head is stored in `prev`.

---

# 🔥 Key Insight

Think of the problem as:

```text
Find Middle

↓

Reverse Second Half

↓

Compare Both Halves
```

instead of trying to compare from both ends directly.

---

# 🎯 Interview One-Liner

Find the middle of the linked list, reverse the second half, and compare both halves node by node to achieve **O(n)** time and **O(1)** space.

---

# 💻 Code (C++)

```cpp
class Solution {
public:

    ListNode* reverse(ListNode* head)
    {
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

    bool isPalindrome(ListNode* head) {

        ListNode* slow = head;
        ListNode* fast = head;

        while(fast && fast->next)
        {
            slow = slow->next;
            fast = fast->next->next;
        }

        ListNode* second = reverse(slow);

        while(second)
        {
            if(head->val != second->val)
                return false;

            head = head->next;
            second = second->next;
        }

        return true;
    }
};
```

---

# 📚 Pattern Recognition

If the problem says:

```text
Compare First Half

Compare Symmetric Nodes

Check Palindrome

Constant Extra Space
```

Think:

```text
Fast & Slow Pointer

↓

Reverse Second Half

↓

Compare Both Halves
```

This pattern is reused in:

```text
234 Palindrome Linked List

143 Reorder List

Various Half-Reversal Linked List Problems
```
