| Topic | Difficulty | Pattern                    |
| ----- | ---------- | -------------------------- |
| Stack | Medium     | Monotonic Increasing Stack |

---

## 📌 Problem Description

For every element in the array, find the first smaller element on its right side.

If no smaller element exists, return:

```text
-1
```

---

## 🧠 Intuition

An element cannot know its Next Smaller Element until a smaller number appears on its right.

Therefore, elements whose answers are still unknown must wait.

A stack is used to store these unresolved elements.

When a smaller element arrives, it resolves one or more waiting elements.

---

## 🎯 Pattern Recognition

Keywords:

* Next Smaller Element
* First Smaller on Right
* Previous Smaller Element
* Histogram Problems
* Monotonic Stack

Key Idea:

```text
Current smaller element resolves larger waiting elements.
```

---

## ⚙️ Approach

### Step 1

Maintain a stack storing indices.

### Step 2

Traverse the array from left to right.

### Step 3

Whenever:

```cpp
arr[i] < arr[st.top()]
```

the current element becomes the Next Smaller Element of the stack top.

Store:

```cpp
ans[st.top()] = arr[i];
```

and pop.

### Step 4

Continue while the current element can resolve more elements.

### Step 5

Push the current index into the stack.

### Step 6

Elements remaining in the stack have no smaller element on their right.

Their answer remains:

```text
-1
```

---

## 🧪 Dry Run

Input:

```text
[4,8,5,2,25]
```

Initial:

```text
ans = [-1,-1,-1,-1,-1]
```

---

### Read 4

Stack:

```text
[0]
```

---

### Read 8

```text
8 < 4 ?
No
```

Stack:

```text
[0,1]
```

---

### Read 5

```text
5 < 8
```

Therefore:

```text
NSE(8) = 5
```

Update:

```text
ans[1] = 5
```

Stack:

```text
[0]
```

Push current index.

Stack:

```text
[0,2]
```

---

### Read 2

```text
2 < 5
```

Therefore:

```text
NSE(5) = 2
```

Pop.

Again:

```text
2 < 4
```

Therefore:

```text
NSE(4) = 2
```

Pop.

Push current index.

Stack:

```text
[3]
```

---

### Read 25

```text
25 < 2 ?
No
```

Push.

Stack:

```text
[3,4]
```

---

End of traversal.

Remaining elements:

```text
2
25
```

have no smaller element.

Final:

```text
[2,5,2,-1,-1]
```

---

## 📚 Stack Visualization

Input:

```text
4 8 5 2 25
```

```text
Read 4

Stack:
4
```

```text
Read 8

Stack:
Top
8
4
```

```text
Read 5

5 resolves 8

Stack:
5
4
```

```text
Read 2

2 resolves 5
2 resolves 4

Stack:
2
```

---

## ⚠️ Common Mistakes

### 1. Using Values Instead of Indices

Wrong:

```cpp
stack<int> st;
```

and storing values when duplicates are possible.

Example:

```text
[5,2,5]
```

Cannot distinguish between the two 5s.

Use:

```cpp
stack<int> st;
```

to store indices.

---

### 2. Comparing with Index

Wrong:

```cpp
arr[i] < st.top()
```

`st.top()` is an index.

Correct:

```cpp
arr[i] < arr[st.top()]
```

---

### 3. Using HashMap

HashMap works only when values are unique.

For generic NGE/NSE problems:

```cpp
vector<int> ans(n,-1);
```

is the standard approach.

---

### 4. Using if Instead of while

Wrong:

```cpp
if(...)
```

One element can resolve multiple waiting elements.

Example:

```text
4 8 5 2
```

When 2 arrives:

```text
2 resolves 5
2 resolves 4
```

Need:

```cpp
while(...)
```

---

## 🔁 Similar Problems

* Next Greater Element
* Daily Temperatures
* Online Stock Span
* Largest Rectangle in Histogram
* Previous Smaller Element

---

## 🎯 Key Takeaways

* Stack stores unresolved indices.
* Current smaller element resolves larger elements.
* Use indices to handle duplicates safely.
* One element may resolve multiple elements.
* Monotonic Stack is the core pattern.

---

## 💻 Implementation (C++)

```cpp
class Solution {
public:
    vector<int> nextSmallerEle(vector<int>& arr) {

        int n = arr.size();

        vector<int> ans(n, -1);
        stack<int> st;

        for(int i = 0; i < n; i++) {

            while(!st.empty() && arr[i] < arr[st.top()]) {
                ans[st.top()] = arr[i];
                st.pop();
            }

            st.push(i);
        }

        return ans;
    }
};
```

---

## ⏱️ Complexity Analysis

### Time Complexity

```text
O(n)
```

Each index is pushed and popped at most once.

---

### Space Complexity

```text
O(n)
```

For the stack and answer array.
