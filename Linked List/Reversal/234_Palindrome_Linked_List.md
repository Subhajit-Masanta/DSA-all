# 234. Palindrome Linked List

## Problem

Given the head of a singly linked list, return:

```text
true  -> if linked list is a palindrome

false -> otherwise
```

A palindrome reads the same forward and backward.

---

# Example 1

Input

```text
1 -> 2 -> 2 -> 1
```

Output

```text
true
```

---

# Example 2

Input

```text
1 -> 2
```

Output

```text
false
```

---

# Brute Force Approach

Store all node values inside a vector.

```cpp
vector<int> arr;
```

Example

```text
1 -> 2 -> 2 -> 1

↓

[1,2,2,1]
```

Then perform a normal palindrome check using two pointers.

---

## Complexity

```text
Time  : O(n)

Space : O(n)
```

Accepted but does not satisfy the follow-up.

---

# Optimal Approach

This problem combines two previously learned patterns.

```text
876 Middle of Linked List

+

206 Reverse Linked List
```

---

# Core Insight

A palindrome is symmetric.

Example

```text
1 -> 2 -> 2 -> 1
```

Split at middle

```text
1 -> 2 | 2 -> 1
```

Reverse second half

```text
1 -> 2 | 1 -> 2
```

Now compare both halves.

```text
1 == 1

2 == 2
```

Palindrome.

---

# Pattern Used

## Step 1

Find middle using

```text
Fast & Slow Pointer
```

---

## Step 2

Reverse second half using

```text
prev
curr
next
```

pattern.

---

## Step 3

Compare first half and reversed second half.

---

# Find Middle

```cpp
ListNode* slow = head;
ListNode* fast = head;

while(fast && fast->next)
{
    slow = slow->next;
    fast = fast->next->next;
}
```

After loop:

```text
slow
```

points to the middle node.

---

# Example

Input

```text
1 -> 2 -> 2 -> 1
```

After loop

```text
1 -> 2 -> 2 -> 1
          ^
         slow
```

---

# Reverse Second Half

Reverse from

```text
slow
```

onwards.

```text
2 -> 1

↓

1 -> 2
```

Store result in

```cpp
second
```

---

# Important Observation

Reversing changes the original links.

Before reverse

```text
1 -> 2 -> 2 -> 1 -> NULL
```

After reverse

```text
head

↓

1 -> 2 -> 2 -> NULL


second

↓

1 -> 2 -> NULL
```

Notice:

The second half is now disconnected and reversed.

---

# Why Compare Using while(second)?

Wrong

```cpp
while(head)
```

Because

```text
head list
```

may continue longer than

```text
second list
```

Eventually

```cpp
second = nullptr;
```

and

```cpp
second->val
```

causes a crash.

---

Correct

```cpp
while(second)
```

Compare only while reversed second half exists.

---

# Dry Run

Input

```text
1 -> 2 -> 2 -> 1
```

Middle

```text
1 -> 2 | 2 -> 1
```

Reverse second half

```text
1 -> 2
```

Compare

```text
1 == 1

2 == 2
```

All equal.

Return

```text
true
```

---

# Odd Length Example

Input

```text
1 -> 2 -> 3 -> 2 -> 1
```

Middle

```text
3
```

Reverse from middle

```text
1 -> 2 -> 3
```

Compare

```text
1 == 1

2 == 2

3 == 3
```

Palindrome.

---

# Complexity

```text
Find Middle      O(n)

Reverse Half     O(n)

Compare          O(n)

Total            O(n)

Space            O(1)
```

---

# Common Mistakes

### Mistake 1

```cpp
while(head)
```

Wrong.

Use

```cpp
while(second)
```

---

### Mistake 2

Forgetting

```cpp
return prev;
```

inside reverse function.

---

### Mistake 3

Calling

```cpp
reverse(slow);
```

without storing returned head.

Correct

```cpp
ListNode* second = reverse(slow);
```

---

### Mistake 4

Comparing node addresses.

Wrong

```cpp
if(head == second)
```

We must compare values.

Correct

```cpp
if(head->val != second->val)
```

---

# Pattern Recognition

This problem is a combination of:

```text
876 Middle of Linked List

+

206 Reverse Linked List
```

Whenever a problem asks:

```text
Compare first half with second half
```

think:

```text
Find Middle

↓

Reverse Second Half

↓

Compare
```

---

# Interview One-Liner

> Find the middle of the linked list, reverse the second half, then compare both halves node by node in O(n) time and O(1) space.

---

# Final Code

```cpp
class Solution {
public:

    ListNode* reverse(ListNode* slow)
    {
        ListNode* prev = nullptr;
        ListNode* curr = slow;

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

# Notebook One-Liner

> Find the middle using Fast & Slow pointers, reverse the second half of the linked list, and compare both halves node by node.
