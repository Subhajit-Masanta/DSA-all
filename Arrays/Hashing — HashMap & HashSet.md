# Hashing — HashMap & HashSet

> **EY Placement / Interview Prep — C++**

---

## 1. Why Do We Need Hashing?

Suppose we have:

```text
[10, 25, 7, 42, 15]
```

To check whether `42` exists:

* Array → search one by one → **O(n)**
* HashSet → average **O(1)** lookup

### Core Idea

```text
Key
 ↓
Hash Function
 ↓
Bucket
 ↓
Data
```

Hashing is mainly used when we need **fast lookup, insertion, deletion, frequency counting, or membership checking**.

---

# 2. Hash Table

A **Hash Table** stores data using a hash function that maps a key to a bucket.

```text
hash(key) → bucket/index
```

Example:

```text
key = 42
table size = 10

42 % 10 = 2

42 → bucket 2
```

This avoids scanning the entire collection.

---

# 3. Hash Function

A hash function converts a key into a hash value used to determine its bucket.

Conceptually:

```cpp
index = hash(key) % tableSize;
```

A good hash function should:

* Be fast
* Distribute keys reasonably uniformly
* Minimize collisions

---

# 4. Collision

A **collision** occurs when different keys map to the same bucket.

```text
12 % 10 = 2
22 % 10 = 2

12 → bucket 2
22 → bucket 2
```

## Collision Handling

### 1. Separate Chaining

Multiple elements are stored in the same bucket.

```text
Bucket 2
   ↓
12 → 22 → 32
```

### 2. Open Addressing

If a bucket is occupied, find another bucket.

Common methods:

```text
Linear Probing
Quadratic Probing
Double Hashing
```

For EY:

> Know the concept and names. Don't waste excessive time implementing every technique.

---

# 5. HashMap

A HashMap stores:

```text
KEY → VALUE
```

Example:

```text
101 → "Subh"
102 → "Rahul"
103 → "Amit"
```

C++:

```cpp
unordered_map<int, string> mp;

mp[101] = "Subh";
mp[102] = "Rahul";
```

### Properties

* Keys are unique
* Values can repeat
* Fast average lookup
* No sorted order

C++ `unordered_map` stores key-value pairs with unique keys and provides average constant-time search, insertion and removal.

---

# 6. HashSet

A HashSet stores **unique values**.

```cpp
unordered_set<int> st;

st.insert(10);
st.insert(20);
st.insert(10);
```

Result:

```text
10
20
```

The second `10` is ignored.

C++ `unordered_set` stores unique objects and provides average constant-time search, insertion and removal.

---

# 7. HashMap vs HashSet

| Feature       | HashMap            | HashSet                 |
| ------------- | ------------------ | ----------------------- |
| Stores        | Key → Value        | Values                  |
| Keys unique   | Yes                | N/A                     |
| Values unique | No                 | Yes                     |
| Main use      | Mapping / counting | Membership / uniqueness |
| C++           | `unordered_map`    | `unordered_set`         |

### Mental Rule

```text
Need X → Y?
      ↓
unordered_map

Need "Have I seen X?"
      ↓
unordered_set
```

---

# 8. `unordered_map` vs `map`

|           | `unordered_map`     | `map`         |
| --------- | ------------------- | ------------- |
| Structure | Hash table          | Balanced BST  |
| Search    | Average O(1)        | O(log n)      |
| Insert    | Average O(1)        | O(log n)      |
| Delete    | Average O(1)        | O(log n)      |
| Sorted    | ❌                   | ✅             |
| Order     | No guaranteed order | Sorted by key |

### Use `unordered_map`

When:

```text
Fast lookup is important
Order does not matter
```

### Use `map`

When:

```text
Need sorted keys
Need ordered traversal
Need range-based operations
```

---

# 9. `unordered_set` vs `set`

|            | `unordered_set` | `set`        |
| ---------- | --------------- | ------------ |
| Structure  | Hash table      | Balanced BST |
| Search     | Average O(1)    | O(log n)     |
| Insert     | Average O(1)    | O(log n)     |
| Delete     | Average O(1)    | O(log n)     |
| Sorted     | ❌               | ✅            |
| Duplicates | ❌               | ❌            |

---

# 10. Complexity

### HashMap / HashSet

```text
Average:
Search   → O(1)
Insert   → O(1)
Delete   → O(1)
```

### Worst Case

```text
Search   → O(n)
Insert   → O(n)
Delete   → O(n)
```

Why?

Because collisions can cause many elements to end up in the same bucket.

C++ reference documentation explicitly describes unordered-container lookup as constant on average and linear in the worst case.

### Space

```text
O(n)
```

---

# 11. Frequency Map ⭐⭐⭐⭐⭐

One of the most important hashing patterns.

Given:

```text
[1, 2, 2, 3, 3, 3]
```

Count frequency:

```cpp
unordered_map<int, int> freq;

for (int x : arr) {
    freq[x]++;
}
```

Result:

```text
1 → 1
2 → 2
3 → 3
```

### Used for

* Frequency counting
* Duplicate detection
* Character counting
* Anagrams
* Most frequent element
* Counting occurrences

---

# 12. HashSet — "Seen Before?" Pattern ⭐⭐⭐⭐⭐

```cpp
unordered_set<int> st;

for (int x : arr) {

    if (st.count(x)) {
        // duplicate found
    }

    st.insert(x);
}
```

Mental pattern:

```text
"Have I seen this before?"
            ↓
      unordered_set
```

---

# 13. Two Sum — HashMap Pattern ⭐⭐⭐⭐⭐

Array:

```text
[2, 7, 11, 15]
target = 9
```

Instead of checking every pair:

```text
O(n²)
```

Use a HashMap:

```cpp
unordered_map<int, int> mp;

for (int i = 0; i < nums.size(); i++) {

    int need = target - nums[i];

    if (mp.count(need)) {
        // pair found
    }

    mp[nums[i]] = i;
}
```

Complexity:

```text
Time  → O(n) average
Space → O(n)
```

### Key idea

```text
Current value
     +
Need previous value
     ↓
Check HashMap
```

---

# 14. C++ `unordered_map` Syntax

```cpp
#include <unordered_map>

unordered_map<int, int> mp;
```

### Insert / Update

```cpp
mp[10] = 50;
```

```cpp
mp.insert({10, 50});
```

### Find

```cpp
if (mp.find(10) != mp.end()) {
    // exists
}
```

### Count

```cpp
if (mp.count(10)) {
    // exists
}
```

### Delete

```cpp
mp.erase(10);
```

### Size

```cpp
mp.size();
```

---

# 15. Important `operator[]` Trap ⭐⭐⭐⭐

```cpp
unordered_map<int, int> mp;

cout << mp[10];
```

If `10` does not exist, `mp[10]` **creates the key**.

For `int`:

```text
10 → 0
```

Therefore:

### For existence checking

Prefer:

```cpp
mp.find(10)
```

or:

```cpp
mp.count(10)
```

Don't use:

```cpp
mp[10]
```

just to check whether the key exists.

---

# 16. C++ `unordered_set` Syntax

```cpp
#include <unordered_set>

unordered_set<int> st;
```

### Insert

```cpp
st.insert(10);
```

### Search

```cpp
if (st.find(10) != st.end()) {
    // exists
}
```

### Count

```cpp
if (st.count(10)) {
    // exists
}
```

### Delete

```cpp
st.erase(10);
```

### Size

```cpp
st.size();
```

---

# 17. When Is Hashing Better?

Use hashing when the problem requires:

```text
✓ Fast membership checking
✓ Fast lookup
✓ Frequency counting
✓ Duplicate detection
✓ Previous-element tracking
✓ Key → value mapping
✓ Pair lookup
✓ Grouping
```

### Problem keywords

If you see:

```text
"frequency"
"duplicate"
"unique"
"seen before"
"exists"
"count occurrences"
"find pair"
"lookup"
"mapping"
```

Think:

```text
HASHING
```

---

# 18. When NOT to Use Hashing?

Don't automatically use hashing.

Use `map` / `set` when you need:

```text
✓ Sorted elements
✓ Ordered traversal
✓ Minimum/maximum by key
✓ Range queries
```

Example:

```text
Need smallest key ≥ X
        ↓
set / map
```

Not:

```text
unordered_set / unordered_map
```

---

# 19. Hashing vs Sorting

### Problem

Find duplicates.

### Sorting

```cpp
sort(arr.begin(), arr.end());
```

Then compare adjacent elements.

```text
Time  → O(n log n)
Space → depends on sorting/implementation
```

### HashSet

```cpp
unordered_set<int> st;
```

```text
Average Time → O(n)
Space        → O(n)
```

### Trade-off

```text
Hashing
→ Faster average lookup
→ Extra memory
→ No ordering

Sorting
→ Slower asymptotically
→ Gives sorted data
→ May use less auxiliary memory
```

---

# 20. Load Factor

### Definition

```text
Load Factor = Number of elements / Number of buckets
```

Example:

```text
Elements = 6
Buckets  = 10

Load Factor = 6 / 10 = 0.6
```

Higher load factor generally means more elements per bucket and potentially more collisions.

C++ unordered containers expose `load_factor`, `max_load_factor`, `rehash`, and `reserve` for hash-table capacity management.

---

# 21. Rehashing

When the hash table needs more buckets:

```text
Old table
   ↓
Allocate larger bucket array
   ↓
Recalculate bucket placement
   ↓
Move elements
```

This is called:

```text
REHASHING
```

Why?

```text
Too many elements
      ↓
Higher load factor
      ↓
More collisions
      ↓
Resize / rehash
```

Know the concept for EY. You don't need to memorize implementation internals.

---

# 22. Hashing Trade-off

Hashing is essentially:

```text
TIME ↔ SPACE
```

Example:

### Without HashMap

```text
Two Sum
→ O(n²)
```

### With HashMap

```text
Two Sum
→ O(n) average
→ O(n) extra space
```

So:

> We use additional memory to reduce lookup time.

---

# 23. Common EY Coding Questions

### Must Practice ⭐⭐⭐⭐⭐

```text
1. Frequency of elements
2. Frequency of characters
3. Find duplicates
4. Remove duplicates
5. Check if two arrays have common elements
6. Two Sum
7. First non-repeating character
8. Valid Anagram
9. Intersection of two arrays
10. Count pairs with given sum
```

### Should Practice ⭐⭐⭐⭐

```text
11. Longest consecutive sequence
12. Majority element using HashMap
13. Group elements by frequency
14. Count occurrences in a range
15. Subarray sum using HashMap
16. Prefix Sum + HashMap
```

EY interview reports have included hashing-based coding questions, including HashMap/HashSet questions, so these patterns are directly relevant to the preparation rather than being random LeetCode topics.

---

# 24. EY Priority

## ⭐⭐⭐⭐⭐ Must Know

```text
Hashing concept
HashMap
HashSet
unordered_map
unordered_set
Frequency Map
Duplicate detection
Two Sum
map vs unordered_map
set vs unordered_set
Average vs worst-case complexity
```

## ⭐⭐⭐⭐ Know

```text
Hash function
Collision
Separate chaining
Open addressing
Load factor
Rehashing
```

## ⭐⭐ Don't Overinvest

```text
Advanced custom hash functions
Complex collision implementations
Production-grade hash table implementation
```

---

# 25. Interview Quick Answers

### What is Hashing?

> Hashing is a technique that maps a key to a bucket using a hash function, allowing average O(1) lookup, insertion and deletion.

### Why use Hashing?

> To perform fast lookup and membership operations without scanning the entire collection.

### What is a collision?

> When two different keys map to the same bucket.

### HashMap vs HashSet?

> HashMap stores key-value pairs, while HashSet stores unique values.

### `map` vs `unordered_map`?

> `map` maintains sorted keys with O(log n) operations, while `unordered_map` uses hashing and provides average O(1) operations but no ordering.

### Why can HashMap become O(n)?

> Due to collisions causing many elements to be placed in the same bucket.

### When would you choose HashSet?

> When I only need to know whether an element exists or whether values are unique.

### When would you choose HashMap?

> When I need to associate a key with information such as a frequency, index, or value.

---

# 26. Final Mental Model

```text
                    HASHING
                       │
          ┌────────────┴────────────┐
          ↓                         ↓
      HashMap                   HashSet
    key → value               unique values
          │                         │
          ↓                         ↓
   Frequency / Index          Seen / Duplicate
   Mapping / Lookup           Membership
          │                         │
          └────────────┬────────────┘
                       ↓
                  Average O(1)
                       │
              No ordering guaranteed
```

### The 5-second decision rule

```text
Need KEY → VALUE?
        → unordered_map

Need FREQUENCY?
        → unordered_map<X, int>

Need "Have I seen X?"
        → unordered_set

Need UNIQUE elements?
        → unordered_set

Need SORTED / ORDERED data?
        → map / set
```

**For EY, this is the theory you should actually remember.** The next step should be **HashMap/HashSet coding practice**, not another giant theory dump.
