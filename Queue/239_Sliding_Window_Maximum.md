# Sliding Window Maximum (LeetCode 239)

| Topic         | Difficulty | Pattern         |
| ------------- | ---------- | --------------- |
| Queue / Deque | Hard       | Monotonic Deque |

---

# 📌 Problem Description

Given an array `nums` and a window size `k`, find the **maximum element** in every sliding window.

Example:

```text
nums = [1,3,-1,-3,5,3,6,7]
k = 3

Windows

[1 3 -1] -3 5 3 6 7 → 3
1 [3 -1 -3] 5 3 6 7 → 3
1 3 [-1 -3 5] 3 6 7 → 5
1 3 -1 [-3 5 3] 6 7 → 5
1 3 -1 -3 [5 3 6] 7 → 6
1 3 -1 -3 5 [3 6 7] → 7

Answer = [3,3,5,5,6,7]
```

---

# 🧠 Intuition

Brute force checks every window separately.

```text
Time = O(n × k)
```

Too slow.

Instead, maintain only the **useful candidates** for the maximum.

A **Monotonic Deque** stores indices in **decreasing order of values**.

The front of the deque always contains the index of the maximum element.

---

# 🎯 Pattern Recognition

Keywords:

* Sliding Window
* Maximum / Minimum
* Queue
* Deque
* Monotonic Queue

Whenever a problem asks for **maximum/minimum in every window**, think **Monotonic Deque**.

---

# 💡 Why Store Indices Instead of Values?

Store **indices**, not values.

Reason:

* Need to know whether an element has moved outside the current window.
* Duplicate values may exist.
* Values alone cannot determine if an element is still inside the window.

Example:

```text
nums = [5,5,5]

Store:

0 1 2
```

Instead of

```text
5 5 5
```

---

# 🔑 Core Idea

For every index:

### Step 1

Remove indices that are outside the current window.

```text
left = right - k + 1

if deque.front() < left

remove it
```

---

### Step 2

Remove all smaller elements from the back.

```text
Current = nums[right]

While

nums[dq.back()] < nums[right]

remove back
```

These smaller elements can never become the maximum again.

---

### Step 3

Push the current index.

```cpp
dq.push_back(right);
```

---

### Step 4

If the first complete window has formed

```text
right >= k-1
```

the answer is

```cpp
nums[dq.front()]
```

---

# 🧪 Dry Run

```text
nums = [1,3,-1,-3,5]

k = 3
```

Initially

```text
Deque = []
Answer = []
```

---

### right = 0

```text
Deque

1
```

No complete window yet.

---

### right = 1

3 is larger than 1.

Remove 1.

```text
Deque

3
```

---

### right = 2

Insert -1.

```text
Deque

3 -1
```

Window completed.

Maximum

```text
3
```

Answer

```text
[3]
```

---

### right = 3

Current window:

```text
[3 -1 -3]
```

Insert -3.

```text
Deque

3 -1 -3
```

Maximum

```text
3
```

Answer

```text
[3,3]
```

---

### right = 4

Current = 5

Remove all smaller elements.

```text
3
-1
-3
```

Deque becomes

```text
5
```

Maximum

```text
5
```

Answer

```text
[3,3,5]
```

---

# ⚠️ Common Mistakes

### 1. Storing Values Instead of Indices

Wrong:

```cpp
deque<int> dq;

dq.push_back(nums[i]);
```

Correct:

```cpp
dq.push_back(i);
```

---

### 2. Forgetting to Remove Out-of-Window Indices

Always check:

```cpp
dq.front() < left
```

before processing the current element.

---

### 3. Using `if` Instead of `while`

Wrong:

```cpp
if(nums[dq.back()] < nums[right])
```

Correct:

```cpp
while(nums[dq.back()] < nums[right])
```

There may be multiple smaller elements.

---

### 4. Adding Answer Too Early

Only after the first complete window:

```cpp
right >= k-1
```

---

# 🎯 Key Takeaways

* Store **indices**, not values.
* Front of deque always stores the maximum.
* Deque is maintained in **decreasing order of values**.
* Remove expired indices from the front.
* Remove smaller elements from the back.
* Every element enters and leaves the deque at most once.

---

# 💻 Implementation (C++)

```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {

        vector<int> ans;
        deque<int> dq;

        for (int right = 0; right < nums.size(); right++) {

            int left = right - k + 1;

            while (!dq.empty() && left > dq.front()) {
                dq.pop_front();
            }

            while (!dq.empty() && nums[dq.back()] < nums[right]) {
                dq.pop_back();
            }

            dq.push_back(right);

            if (right >= k - 1) {
                ans.push_back(nums[dq.front()]);
            }
        }

        return ans;
    }
};
```

---

# ⏱️ Complexity Analysis

| Operation | Complexity |
| --------- | ---------- |
| Time      | **O(n)**   |
| Space     | **O(k)**   |

### Why O(n)?

Each index:

* enters the deque **once**
* leaves the deque **once**

Therefore total deque operations are at most **2n**.

---

# 📚 Pattern Summary

```text
Sliding Window Maximum

↓

Deque stores indices

↓

Remove expired indices

↓

Remove smaller elements

↓

Push current index

↓

Front always gives maximum
```

---

# 🚀 Placement Takeaways

* One of the most important **Deque** interview problems.
* Introduces the **Monotonic Queue** pattern.
* Store **indices**, not values.
* Front always represents the maximum element of the current window.
* Time complexity improves from **O(n × k)** to **O(n)**.
