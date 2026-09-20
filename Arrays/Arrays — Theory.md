# Arrays — Theory Required for EY

> **EY Placement / Interview Prep — C++**

---

## 1. What is an Array?

An **array** is a collection of elements of the **same data type** stored in **contiguous memory locations**.

```cpp
int arr[5] = {10, 20, 30, 40, 50};
```

Memory conceptually:

```text
1000 → 10
1004 → 20
1008 → 30
1012 → 40
1016 → 50
```

Because elements are contiguous, we can directly calculate the address of any element.

---

# 2. Why Do We Need Arrays?

Arrays are useful when:

* Number of elements is known/fixed
* We need fast indexed access
* Elements are naturally sequential
* We frequently access elements by position

Example:

```cpp
arr[3]
```

Directly accesses the 4th element.

### Access Complexity

```text
arr[i] → O(1)
```

This is the biggest advantage of arrays.

---

# 3. Array Indexing

For an array:

```cpp
int arr[5];
```

Valid indexes:

```text
0  1  2  3  4
```

If base address is `B` and each element takes `S` bytes:

```text
Address(arr[i]) = B + i × S
```

This is why random access is **O(1)**.

---

# 4. Array Time Complexities

| Operation           | Complexity |
| ------------------- | ---------: |
| Access by index     |   **O(1)** |
| Update by index     |   **O(1)** |
| Search              |   **O(n)** |
| Insert at end       |      O(1)* |
| Insert at beginning |       O(n) |
| Insert in middle    |       O(n) |
| Delete at beginning |       O(n) |
| Delete in middle    |       O(n) |
| Delete at end       |      O(1)* |

`*` Assuming there is available space and no resizing is required.

---

# 5. Why Is Array Access O(1)?

Because the address is calculated directly:

```text
base address + index × element size
```

There is **no need to traverse previous elements**.

Therefore:

```cpp
arr[500];
```

doesn't mean:

```text
0 → 1 → 2 → ... → 500
```

It jumps directly to the required memory location.

---

# 6. Why Is Insertion at the Beginning O(n)?

Suppose:

```text
[10, 20, 30, 40]
```

Insert `5` at index `0`.

Elements must shift:

```text
[10, 20, 30, 40]
 ↓
[5, 10, 20, 30, 40]
```

Approximately `n` elements may need to move.

Therefore:

```text
O(n)
```

Same reasoning applies to deletion from the beginning/middle.

---

# 7. Static vs Dynamic Arrays

## Static Array

Size is fixed.

```cpp
int arr[100];
```

You cannot increase its capacity directly.

---

## Dynamic Array

Can grow when required.

In C++:

```cpp
vector<int> arr;
```

`vector` is the main dynamic-array structure you should know.

---

# 8. Array vs Vector

|               | Array        | `vector`      |
| ------------- | ------------ | ------------- |
| Size          | Fixed        | Dynamic       |
| Random access | O(1)         | O(1)          |
| Memory        | Contiguous   | Contiguous    |
| Resize        | ❌            | ✅             |
| C++           | `int arr[5]` | `vector<int>` |

### Why use `vector`?

When the number of elements is not known beforehand or may change.

```cpp
vector<int> nums;
nums.push_back(10);
nums.push_back(20);
```

---

# 9. Vector `push_back()`

Normally:

```text
push_back() → O(1) amortized
```

Sometimes the vector runs out of capacity.

Then it may:

```text
Allocate larger memory
        ↓
Copy/move elements
        ↓
Release old memory
```

That particular operation can be **O(n)**.

Therefore:

> `push_back()` is **amortized O(1)**, not guaranteed O(1) for every individual operation.

---

# 10. Array vs Linked List

Very common interview comparison.

|                      | Array      | Linked List    |
| -------------------- | ---------- | -------------- |
| Memory               | Contiguous | Non-contiguous |
| Random access        | O(1)       | O(n)           |
| Search               | O(n)       | O(n)           |
| Insert beginning     | O(n)       | O(1)           |
| Delete beginning     | O(n)       | O(1)           |
| Cache locality       | Better     | Worse          |
| Extra pointer memory | No         | Yes            |

### Choose Array when:

```text
Need fast index access
Need contiguous data
Mostly reading/accessing elements
```

### Choose Linked List when:

```text
Frequent insertion/deletion
Especially at known positions/nodes
Random access isn't important
```

---

# 11. Array Search

## Linear Search

Works on any array.

```cpp
for (int i = 0; i < n; i++) {
    if (arr[i] == target)
        return i;
}
```

Complexity:

```text
Time  → O(n)
Space → O(1)
```

---

# 12. Binary Search

Works when the array is **sorted**.

Example:

```text
[2, 5, 8, 12, 17, 20]
```

Instead of checking every element, repeatedly divide the search range.

```text
Time  → O(log n)
Space → O(1) iterative
```

### Important

```text
Unsorted array → Linear Search
Sorted array   → Binary Search
```

EY technical interviews have specifically reported candidates being asked to write/explain binary search.

---

# 13. Two-Pointer Technique ⭐⭐⭐⭐⭐

Important array pattern.

Usually used when:

* Array is sorted
* We need a pair
* We need to compare elements from both ends
* We need in-place modification

Example:

```text
[1, 2, 3, 4, 6]
target = 6
```

Use:

```text
left  → 1
right → 6
```

Move pointers based on the sum.

Typical complexity:

```text
O(n)
```

---

# 14. Sliding Window ⭐⭐⭐⭐⭐

Used for problems involving **contiguous subarrays/substrings**.

Example:

```text
Find maximum sum of subarray of size k
```

Instead of calculating every window from scratch:

```text
O(n × k)
```

Maintain a moving window:

```text
[1 2 3] 4 5
  [2 3 4] 5
    [3 4 5]
```

Typical complexity:

```text
O(n)
```

You have already covered Sliding Window, so for EY this only needs revision, not another deep theory session.

---

# 15. Prefix Sum ⭐⭐⭐⭐

Used when we repeatedly need subarray/range sums.

Array:

```text
[2, 4, 1, 5]
```

Prefix:

```text
[2, 6, 7, 12]
```

Formula:

```text
prefix[i] = prefix[i-1] + arr[i]
```

Then a range sum can be calculated quickly.

For:

```text
sum(l...r)
```

use:

```text
prefix[r] - prefix[l-1]
```

Typical:

```text
Build → O(n)
Range query → O(1)
```

---

# 16. Kadane's Algorithm ⭐⭐⭐⭐

Used for:

> Maximum sum contiguous subarray.

Example:

```text
[-2, 1, -3, 4, -1, 2, 1, -5, 4]
```

Maximum subarray:

```text
[4, -1, 2, 1]
```

Sum:

```text
6
```

Complexity:

```text
Time  → O(n)
Space → O(1)
```

Know the idea and implementation.

---

# 17. Sorting Basics

For EY, know the basic sorting concepts and at least one implementation.

Important:

```text
Bubble Sort
Selection Sort
Insertion Sort
```

Know:

* Basic idea
* Time complexity
* When it works
* Stable vs unstable at a basic level

### Complexity

| Algorithm |  Best | Average | Worst |
| --------- | ----: | ------: | ----: |
| Bubble    | O(n)* |   O(n²) | O(n²) |
| Selection | O(n²) |   O(n²) | O(n²) |
| Insertion |  O(n) |   O(n²) | O(n²) |

`*` With an optimized bubble sort that stops when no swaps occur.

EY coding reports have explicitly included **bubble sort on a 1D array** and other array manipulation questions.

---

# 18. 2D Arrays / Matrix

Know basic matrix operations.

```cpp
int matrix[3][3];
```

Be comfortable with:

```text
Traversal
Input/output
Row/column operations
Transpose
Diagonal traversal
Matrix addition
Matrix multiplication
```

Matrix multiplication has appeared in an EY coding assessment report.

---

# 19. Common Array Patterns

Recognize these patterns:

```text
Array
 │
 ├── Linear Search
 ├── Binary Search
 ├── Two Pointers
 ├── Sliding Window
 ├── Prefix Sum
 ├── Hashing
 ├── Sorting
 ├── Kadane's Algorithm
 └── Matrix
```

The important skill is not memorizing random questions.

It's recognizing:

```text
Problem
   ↓
Pattern
   ↓
Algorithm
```

---

# 20. Array vs Hashing

Important trade-off.

### Search in Array

```text
O(n)
```

### Search using HashSet

```text
Average O(1)
```

So:

```text
Need ordering/index → Array
Need fast membership → HashSet
```

Don't replace arrays with HashSets blindly.

---

# 21. Array vs Binary Search

```text
Unsorted array
     ↓
Linear Search → O(n)
```

```text
Sorted array
     ↓
Binary Search → O(log n)
```

The sorting itself costs:

```text
O(n log n)
```

So don't sort an array just to perform **one** search unless there is another reason to do so.

---

# 22. EY Array Theory Checklist

## ⭐⭐⭐⭐⭐ MUST KNOW

```text
✓ Array definition
✓ Contiguous memory
✓ Indexing
✓ Random access
✓ Array complexities
✓ Linear Search
✓ Binary Search
✓ Two Pointers
✓ Sliding Window
✓ Basic sorting
✓ 2D arrays / matrices
```

## ⭐⭐⭐⭐ SHOULD KNOW

```text
✓ Prefix Sum
✓ Kadane's Algorithm
✓ Array vs Vector
✓ Array vs Linked List
✓ Amortized O(1) push_back
```

## ⭐⭐ LOW PRIORITY

```text
✗ Advanced segment trees
✗ Fenwick trees
✗ Sparse tables
✗ Complex range-query structures
✗ Advanced array mathematics
```

---

# 23. EY Array Questions to Practice

Based on reported EY assessments/interviews and the current EY coding-question coverage:

### Must Do

```text
1. Find maximum/minimum
2. Reverse an array
3. Linear search
4. Binary search
5. Find duplicate
6. Remove duplicates
7. Move zeroes
8. Rotate array
9. Two Sum
10. Maximum subarray — Kadane
11. Merge two sorted arrays
12. Second largest element
13. Missing number
14. Best Time to Buy and Sell Stock
15. Sort an array
```

### 2D Array

```text
16. Matrix addition
17. Matrix transpose
18. Matrix multiplication
19. Diagonal traversal
20. Spiral matrix
```

---

# 24. Final EY Mental Model

```text
                    ARRAY
                      │
       ┌──────────────┼──────────────┐
       ↓              ↓              ↓
    Access          Search       Modification
      O(1)             │              │
                       │              │
                ┌──────┴──────┐       │
                ↓             ↓       ↓
             Linear       Binary    Shift
              O(n)       O(log n)   O(n)
                           sorted
```

### Remember these trade-offs:

```text
Array
→ Fast index access
→ Contiguous memory
→ O(1) access
→ O(n) insertion/deletion in middle

HashSet
→ Fast average membership
→ No ordering

Map/Set
→ Ordered
→ O(log n)

Linked List
→ Poor random access
→ Easier insertion/deletion at known nodes
```

---

## EY Bottom Line

You **do not need 50 pages of array theory**.

For EY, get these absolutely solid:

```text
1. Array memory + indexing
2. Complexity of operations
3. Linear vs Binary Search
4. Two Pointers
5. Sliding Window
6. Prefix Sum
7. Kadane
8. Basic Sorting
9. 2D Arrays / Matrix
10. Array vs Vector vs Linked List vs Hashing
```

Reported EY coding rounds have ranged from very easy array problems to array manipulation and matrix multiplication, while recent EY-specific coding lists emphasize common easy/medium array patterns rather than advanced competitive-programming techniques.
