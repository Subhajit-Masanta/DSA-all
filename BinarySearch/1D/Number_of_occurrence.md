# 🔍 Number of Occurrences in Sorted Array

| Topic | Difficulty | Pattern |
|---|---|---|
| Binary Search | Easy | Boundary Binary Search |

## 📌 Problem
Given a sorted array `arr[]` and a `target`, return the number of times the target appears.

Must run in:
```cpp
O(log n)
```

### Examples

```cpp
Input: arr = [1,1,2,2,2,2,3], target = 2
Output: 4
```

```cpp
Input: arr = [1,1,2,2,2,2,3], target = 4
Output: 0
```

```cpp
Input: arr = [8,9,10,12,12,12], target = 12
Output: 3
```

---

## 🧠 Core Insight

This is NOT a new problem.

It is the SAME pattern as:
```cpp
First Occurrence + Last Occurrence
```

Once we find:
```cpp
first index
last index
```

Count becomes:
```cpp
last - first + 1
```

---

## 🔥 Why `+1` ?

Example:
```cpp
arr = [2,2,2,2]
```

Indexes:
```cpp
0 1 2 3
```

First:
```cpp
0
```

Last:
```cpp
3
```

Count:
```cpp
3 - 0 + 1 = 4
```

Without `+1`:
```cpp
3 - 0 = 3
```

which is wrong.

---

## 🧠 Mental Model

Occurrences mean:
```cpp
length of target boundary
```

So:
```cpp
count = last - first + 1
```

---

## ✅ First Occurrence Logic

Goal:
```cpp
smallest valid index
```

When:
```cpp
arr[mid] == target
```

Store answer and:
```cpp
go LEFT
```

Because:
```cpp
another target may exist earlier
```

---

## ✅ Last Occurrence Logic

Goal:
```cpp
largest valid index
```

When:
```cpp
arr[mid] == target
```

Store answer and:
```cpp
go RIGHT
```

Because:
```cpp
another target may exist later
```

---

## ⚠️ Important Edge Case

If:
```cpp
first == -1
```

Then:
```cpp
target does not exist
```

Return:
```cpp
0
```

---

## 🧪 Example

```cpp
arr = [1,1,2,2,2,2,3]
target = 2
```

### First Occurrence
```cpp
first = 2
```

### Last Occurrence
```cpp
last = 5
```

### Count
```cpp
5 - 2 + 1 = 4
```

---

## 🔥 Pattern Recognition

This belongs to:
```cpp
Boundary Binary Search
```

Related Problems:

| Problem | Meaning |
|---|---|
| Lower Bound | first index >= target |
| Upper Bound | first index > target |
| First Occurrence | first index == target |
| Last Occurrence | last index == target |
| Count Occurrences | last - first + 1 |

---

## 💻 Code

```cpp
class Solution {
public:

    int countFreq(vector<int>& arr, int target) {

        int low = 0;
        int high = arr.size() - 1;

        int first = -1;
        int last = -1;

        // First Occurrence
        while(low <= high) {

            int mid = low + (high - low) / 2;

            if(arr[mid] > target) {
                high = mid - 1;
            }

            else if(arr[mid] < target) {
                low = mid + 1;
            }

            else {
                first = mid;
                high = mid - 1;
            }
        }

        // Target not present
        if(first == -1)
            return 0;

        low = 0;
        high = arr.size() - 1;

        // Last Occurrence
        while(low <= high) {

            int mid = low + (high - low) / 2;

            if(arr[mid] > target) {
                high = mid - 1;
            }

            else if(arr[mid] < target) {
                low = mid + 1;
            }

            else {
                last = mid;
                low = mid + 1;
            }
        }

        return last - first + 1;
    }
};
```

---

## ⏱ Complexity

| Type | Complexity |
|---|---|
| Time | O(log n) |
| Space | O(1) |

---

## 📓 Revision Notes

### Pattern
Boundary Binary Search

### First Occurrence
Found target → go LEFT

### Last Occurrence
Found target → go RIGHT

### Count Formula
```cpp
last - first + 1
```

### Core Idea
Search boundaries, not just values

### One-liner
Count occurrences by finding left and right boundaries.
