# 🔍 Lower Bound & Upper Bound

| Topic | Difficulty | Pattern |
|---|---|---|
| Binary Search | Easy | Boundary Binary Search |

## 📌 Problem

### Lower Bound
Find:
```cpp
first index where:
arr[i] >= x
```

If no such index exists:
```cpp
return n
```

---

### Upper Bound
Find:
```cpp
first index where:
arr[i] > x
```

If no such index exists:
```cpp
return n
```

---

# 🧠 Core Insight

These are NOT normal binary search problems.

We are NOT searching:
```cpp
"Does x exist?"
```

We are searching:
```cpp
"Where does condition become TRUE?"
```

Binary Search works because conditions become monotonic:

---

## Lower Bound Pattern

Condition:
```cpp
arr[i] >= x
```

Pattern:
``` id="ekjlwm"
FALSE FALSE FALSE TRUE TRUE TRUE
```

We need:
```cpp
FIRST TRUE
```

---

## Upper Bound Pattern

Condition:
```cpp
arr[i] > x
```

Pattern:
``` id="87h4li"
FALSE FALSE FALSE TRUE TRUE TRUE
```

Again:
```cpp
FIRST TRUE
```

---

# 🔥 VERY IMPORTANT DIFFERENCE

| Concept | Meaning |
|---|---|
| Lower Bound | first >= x |
| Upper Bound | first > x |
| Floor | largest <= x |
| Ceil | smallest >= x |

---

# ⚠️ MOST COMMON CONFUSION

Students think:
```cpp
Lower Bound = Floor
```

❌ WRONG.

---

# ✅ Lower Bound

Find:
```cpp
first arr[i] >= x
```

Searches:
```cpp
LEFTMOST VALID POSITION
```

---

# ✅ Floor

Find:
```cpp
largest arr[i] <= x
```

Searches:
```cpp
RIGHTMOST VALID POSITION
```

Different goals.
Different directions.

---

# 🧠 Mental Model

## Lower Bound
```cpp
Find first valid index
```

So after valid answer:
```cpp
go LEFT
```

---

## Floor
```cpp
Find last valid index
```

So after valid answer:
```cpp
go RIGHT
```

---

# 🔥 Lower Bound Logic

If:
```cpp
arr[mid] >= x
```

Then:
- mid is VALID
- maybe smaller valid index exists

So:
```cpp
ans = mid;
high = mid - 1;
```

---

If:
```cpp
arr[mid] < x
```

Then:
```cpp
mid too small
```

Go RIGHT:
```cpp
low = mid + 1;
```

---

# 🔥 Upper Bound Logic

If:
```cpp
arr[mid] > x
```

Then:
- mid is VALID
- maybe smaller valid index exists

So:
```cpp
ans = mid;
high = mid - 1;
```

---

If:
```cpp
arr[mid] <= x
```

Then:
```cpp
mid too small
```

Go RIGHT:
```cpp
low = mid + 1;
```

---

# 🧪 Example

```cpp
arr = [1,2,5,8,10,10,12,19]
x = 5
```

| Concept | Answer |
|---|---|
| Lower Bound | index 2 |
| Upper Bound | index 3 |
| Floor | 5 |
| Ceil | 5 |

---

# 🧪 Another Example

```cpp
arr = [1,2,5,8,10,10,12,19]
x = 9
```

| Concept | Answer |
|---|---|
| Lower Bound | index of 10 |
| Upper Bound | index of 10 |
| Floor | 8 |
| Ceil | 10 |

---

# ❓ Why Go LEFT Again in Lower Bound?

Example:
```cpp
arr = [1,2,5,5,5,8]
x = 5
```

Suppose:
```cpp
mid = 4
arr[mid] = 5
```

Valid?

✅ YES.

BUT:
```cpp
earlier 5 may exist
```

Lower Bound wants:
```cpp
FIRST valid index
```

So:
```cpp
go LEFT
```

---

# 💻 Lower Bound Code

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

# 💻 Upper Bound Code

```cpp
class Solution {
public:

    int upperBound(vector<int>& arr, int x) {

        int low = 0;
        int high = arr.size() - 1;

        int ans = arr.size();

        while(low <= high) {

            int mid = low + (high - low) / 2;

            if(arr[mid] > x) {

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

| Problem | Goal |
|---|---|
| Lower Bound | First TRUE |
| Upper Bound | First GREATER |
| First Occurrence | First == target |
| Last Occurrence | Last == target |

These are all:
```cpp
Boundary Binary Search
```

---

# ⏱ Complexity

| Type | Complexity |
|---|---|
| Time | O(log n) |
| Space | O(1) |

---

# 📓 Revision Notes

### Lower Bound
```cpp
first >= x
```

### Upper Bound
```cpp
first > x
```

### Key Idea
Search first valid index

### Important Logic
- valid → store ans, go LEFT
- invalid → go RIGHT

### One-liner
Lower/Upper Bound search transition points.
