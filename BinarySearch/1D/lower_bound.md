# 🔍 Lower Bound

| Topic | Difficulty | Pattern |
| :--- | :--- | :--- |
| Arrays | Easy | Binary Search |

---

# 📌 Problem Description

Find the **Lower Bound** of `x` in a sorted array.

### Lower Bound:
> The first index where:
```cpp
arr[i] >= x
```

If no such index exists:
```cpp
return n

Given a sorted array arr[] and an integer x, find the index (0-based) of the largest element in arr[] that is less than or equal to x. This element is called the floor of x. If such an element does not exist, return -1.

Note: In case of multiple occurrences of floor of x, return the index of the last occurrence.
```

---

# 🧠 Core Insight

Lower Bound searches for:
> First element greater than or equal to `x`

This is NOT normal binary search.

You are searching for:
- the **first valid position**
- not exact existence

---

# 🔥 Key Observation

---

# ✅ If `arr[mid] >= x`

Then:
- `mid` can be a possible answer
- BUT maybe a smaller valid index exists on left

So:
```cpp
ans = mid;
high = mid - 1;
```

---

# ✅ If `arr[mid] < x`

Then:
- current element is too small
- lower bound must be on right side

So:
```cpp
low = mid + 1;
```

---

# ⚙️ Algorithm

1. Initialize:
```cpp
low = 0
high = n - 1
ans = n
```

2. While `low <= high`
- Find `mid`
- If `arr[mid] >= x`
    - store answer
    - move left
- Else
    - move right

3. Return `ans`

---

# 🧪 Example 1

Input:
```cpp
arr = [1,2,4,4,5]
x = 4
```

Process:
``` id="hclg6y"
mid = 2 → arr[mid] = 4

4 >= 4
possible answer = 2

go left
```

Output:
```cpp
2
```

---

# 🧪 Example 2

Input:
```cpp
arr = [1,2,4,4,5]
x = 3
```

Process:
``` id="1zy8yj"
mid = 2 → arr[mid] = 4

4 >= 3
possible answer = 2

go left
```

Output:
```cpp
2
```

Because:
```cpp
arr[2] = 4
```

is first element greater than or equal to `3`.

---

# 🧪 Example 3

Input:
```cpp
arr = [1,2,4,4,5]
x = 8
```

No element satisfies:
```cpp
arr[i] >= 8
```

Output:
```cpp
5
```

Meaning:
```cpp
lower bound = n
```

---

# 🔥 VERY IMPORTANT

---

# ❌ Your Code is NOT Lower Bound

Your code:
```cpp
if(x >= arr[mid])
```

This finds:
> Largest element less than or equal to `x`

That is:
# ✅ FLOOR

NOT Lower Bound.

---

# 🧠 Difference

---

# ✅ Lower Bound

Find:
```cpp
first arr[i] >= x
```

Move:
```cpp
if(arr[mid] >= x)
```

---

# ✅ Floor

Find:
```cpp
largest arr[i] <= x
```

Move:
```cpp
if(arr[mid] <= x)
```

---

# ⚠️ Common Confusion

Students confuse:
- Lower Bound
- Upper Bound
- Floor
- Ceil

These are DIFFERENT patterns.

You must know all 4.

---

# 💻 Correct Lower Bound Code

```cpp
class Solution {
public:
    int lowerBound(vector<int>& arr, int x) {

        int low = 0;
        int high = arr.size() - 1;

        int ans = arr.size();

        while(low <= high) {

            int mid = low + (high - low) / 2;

            if(arr[mid] >= x) {
                ans = mid;
                high = mid - 1;
            }
            else {
                low = mid + 1;
            }
        }

        return ans;
    }
};
```

---

# 🔥 Pattern Recognition

Lower Bound =

> First TRUE position

Where condition becomes:
```cpp
arr[i] >= x
```

Binary Search is basically:
> Searching transition point

---

# ⏱ Complexity

- Time: `O(log n)`
- Space: `O(1)`

---

# 📓 Revision Notes

### Pattern:
Binary Search / Lower Bound

### Key Idea:
Find first element greater than or equal to target

### Important Logic:
- valid → store ans, move left
- invalid → move right

### One-liner:
Find first index where condition becomes true.
