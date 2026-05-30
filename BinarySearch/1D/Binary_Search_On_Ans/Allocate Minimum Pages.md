# 🔍 Allocate Minimum Pages

| Topic | Difficulty | Pattern |
|---|---|---|
| Binary Search | Medium | Binary Search on Answers |

## 📌 Problem

Given:

```cpp
arr[i]
```

where:

```cpp
arr[i]
```

represents the number of pages in the `i-th` book.

And:

```cpp
k
```

students.

Allocate books such that:

- Every student gets at least one book.
- Books assigned to a student must be contiguous.
- Every book must be assigned.
- A book cannot be assigned twice.

Return:
# Minimum possible maximum pages assigned to any student.

---

## 🧠 Core Insight

We are NOT searching:

```cpp
student
```

or

```cpp
book index
```

We are searching:

# Maximum pages allowed per student

---

## Example

```cpp
arr = [12,34,67,90]
k = 2
```

Possible allocation:

```cpp
[12,34,67] | [90]
```

Pages:

```cpp
113
90
```

Maximum:

```cpp
113
```

---

Another allocation:

```cpp
[12,34] | [67,90]
```

Pages:

```cpp
46
157
```

Maximum:

```cpp
157
```

---

We want:

```cpp
minimum(maximum pages)
```

Answer:

```cpp
113
```

---

# 🔥 Binary Search Question

Suppose:

```cpp
limit = 120
```

Ask:

# Can all books be allocated using at most k students if no student gets more than 120 pages?

---

## If YES

Maybe smaller limit also works.

```cpp
go LEFT
```

---

## If NO

Limit too small.

```cpp
go RIGHT
```

---

# 🧠 Search Space

Minimum possible answer:

```cpp
max(arr)
```

Because:

```cpp
largest book
```

must be assigned to someone.

---

Maximum possible answer:

```cpp
sum(arr)
```

One student gets all books.

---

Therefore:

```cpp
low = max(arr)
high = sum(arr)
```

---

# ⚙️ Validation Logic

Keep:

```cpp
currentPage
```

representing pages assigned to current student.

And:

```cpp
total
```

representing students used.

---

For each book:

---

### If book fits

```cpp
currentPage + arr[i] <= limit
```

Add book to current student:

```cpp
currentPage += arr[i]
```

---

### Else

Need new student:

```cpp
total++
currentPage = arr[i]
```

Current book becomes first book of new student.

---

At the end:

```cpp
total <= k
```

means allocation is possible.

---

# 🧪 Example

```cpp
arr = [12,34,67,90]
limit = 120
```

---

Student 1:

```cpp
12 + 34 + 67 = 113
```

Next:

```cpp
90
```

would exceed:

```cpp
120
```

New student.

---

Student 2:

```cpp
90
```

Students used:

```cpp
2
```

Valid.

---

# ⚠️ Edge Case

If:

```cpp
k > arr.size()
```

Return:

```cpp
-1
```

Because every student must receive at least one book.

---

# ⚙️ Algorithm

1. If:

```cpp
k > n
```

return:

```cpp
-1
```

2. Search range:

```cpp
max(arr) → sum(arr)
```

3. Binary search on:

```cpp
maximum pages allowed
```

4. Check students needed.

5. If valid:

```cpp
store answer
go LEFT
```

6. Else:

```cpp
go RIGHT
```

---

# 💻 Code

```cpp
class Solution {
public:

    int findmax(vector<int>& weights) {

        int m = weights[0];

        for(int i = 1; i < weights.size(); i++) {
            m = max(m, weights[i]);
        }

        return m;
    }

    int sum(vector<int>& weights) {

        int m = 0;

        for(int i = 0; i < weights.size(); i++) {
            m += weights[i];
        }

        return m;
    }

    bool works(vector<int>& arr, int mid, int k) {

        int pages = mid;

        int total = 1;

        int currentpage = 0;

        int n = arr.size();

        if(n < k) return false;

        for(int i = 0; i < arr.size(); i++) {

            if(currentpage + arr[i] <= pages) {

                currentpage += arr[i];
            }

            else {

                currentpage = arr[i];

                total++;
            }
        }

        return total <= k;
    }

    int findPages(vector<int>& arr, int k) {

        int low = findmax(arr);

        int high = sum(arr);

        int ans = -1;

        while(low <= high) {

            int mid = low + (high - low) / 2;

            if(works(arr, mid, k)) {

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

# ⏱ Complexity

| Type | Complexity |
|---|---|
| Time | O(n × log(sum(arr))) |
| Space | O(1) |

---

# 📓 Revision Notes

### Pattern

Binary Search on Answers

### Search Space

```cpp
max(arr) → sum(arr)
```

### Validation

Greedy student allocation

### If Valid

```cpp
totalStudents <= k
```

go LEFT

### If Invalid

go RIGHT

### One-liner

Binary search the minimum maximum pages that can be assigned to a student.
