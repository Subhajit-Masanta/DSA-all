# 🔍 Floor and Ceil in Sorted Array

| Topic | Difficulty | Pattern |
|---|---|---|
| Binary Search | Easy | Boundary Binary Search |

## 📌 Problem

Given a sorted array `arr[]` and a number `x`:

- **Floor** → largest value `<= x`
- **Ceil** → smallest value `>= x`

If answer does not exist:
```cpp
return -1
```

---

## 🧠 Core Insight

This is NOT normal binary search.

We are searching for:
```cpp
boundary values
```

Difference:

| Concept | Meaning |
|---|---|
| Floor | last valid value |
| Ceil | first valid value |

---

# ✅ Floor

## Definition
```cpp
largest value <= x
```

---

## 🔥 Mental Model

We want:
```cpp
LAST VALID
```

Condition:
```cpp
arr[mid] <= x
```

If valid:
- store answer
- maybe larger valid exists

So:
```cpp
go RIGHT
```

---

## 🧪 Example

```cpp
arr = [1,2,5,8,10,12]
x = 9
```

Possible valid values:
```cpp
1 2 5 8
```

Largest valid:
```cpp
8
```

---

## ⚙️ Floor Code

```cpp
int floorValue(vector<int>& arr, int x) {

    int low = 0;
    int high = arr.size() - 1;

    int ans = -1;

    while(low <= high) {

        int mid = low + (high - low)/2;

        if(arr[mid] <= x) {
            ans = arr[mid];
            low = mid + 1;
        }
        else {
            high = mid - 1;
        }
    }

    return ans;
}
```

---

# ✅ Ceil

## Definition
```cpp
smallest value >= x
```

---

## 🔥 Mental Model

We want:
```cpp
FIRST VALID
```

Condition:
```cpp
arr[mid] >= x
```

If valid:
- store answer
- maybe smaller valid exists

So:
```cpp
go LEFT
```

---

## 🧪 Example

```cpp
arr = [1,2,5,8,10,12]
x = 9
```

Possible valid values:
```cpp
10 12
```

Smallest valid:
```cpp
10
```

---

## ⚙️ Ceil Code

```cpp
int ceilValue(vector<int>& arr, int x) {

    int low = 0;
    int high = arr.size() - 1;

    int ans = -1;

    while(low <= high) {

        int mid = low + (high - low)/2;

        if(arr[mid] >= x) {
            ans = arr[mid];
            high = mid - 1;
        }
        else {
            low = mid + 1;
        }
    }

    return ans;
}
```

---

# 🔥 Lower Bound vs Ceil

Students confuse these constantly.

---

## Lower Bound
Find:
```cpp
first index >= x
```

Returns:
```cpp
INDEX
```

---

## Ceil
Find:
```cpp
smallest value >= x
```

Returns:
```cpp
VALUE
```

---

## Example

```cpp
arr = [1,2,5,8,10]
x = 6
```

Lower Bound:
```cpp
index = 3
```

Ceil:
```cpp
value = 8
```

---

# 🔥 Upper Bound vs Floor

## Upper Bound
Find:
```cpp
first index > x
```

Returns:
```cpp
INDEX
```

---

## Floor
Find:
```cpp
largest value <= x
```

Returns:
```cpp
VALUE
```

---

# 🧠 Direction Logic

| Problem | Valid Condition | Move |
|---|---|---|
| Lower Bound | >= x | LEFT |
| Upper Bound | > x | LEFT |
| Floor | <= x | RIGHT |
| Ceil | >= x | LEFT |

---

# ⚠️ Common Confusions

### ❓ Why Floor goes RIGHT?

Because:
```cpp
we want largest valid value
```

Maybe bigger valid exists.

---

### ❓ Why Ceil goes LEFT?

Because:
```cpp
we want smallest valid value
```

Maybe smaller valid exists.

---

### ❓ Why are Lower Bound and Ceil similar?

Both use:
```cpp
arr[mid] >= x
```

But:
- Lower Bound returns INDEX
- Ceil returns VALUE

---

# ⏱ Complexity

| Type | Complexity |
|---|---|
| Time | O(log n) |
| Space | O(1) |

---

# 📓 Revision Notes

### Floor
largest <= x  
valid → go RIGHT

### Ceil
smallest >= x  
valid → go LEFT

### Core Idea
Search boundary values

### One-liner
Floor = last valid, Ceil = first valid.
