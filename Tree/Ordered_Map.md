# C++ Map (Ordered Map)

---

## 📌 What is a Map?

A **map** is an STL container that stores data as **Key → Value** pairs.

Each **key is unique** and is automatically stored in **sorted order**. Search, insertion, and deletion are all **O(log n)** because `std::map` is typically implemented using a self-balancing Red-Black Tree.

---

## 🧠 Real-Life Example

```text
Roll No.  →  Name

101       →  Rahul
102       →  Aman
103       →  Priya
```

Here,

* **Key** = Roll Number
* **Value** = Name

---

## 📚 Syntax

```cpp
#include <map>

map<Key, Value> mp;
```

Example

```cpp
map<int, string> mp;
```

---

## 🎯 Why Use a Map?

Unlike arrays, keys can be anything.

```cpp
mp[10] = "Ten";
mp[-5] = "Minus Five";
mp[1000] = "Thousand";
```

---

## 📦 Insertion

### Method 1 (Most Common)

```cpp
mp[1] = "One";
mp[2] = "Two";
```

---

### Method 2

```cpp
mp.insert({3,"Three"});
```

---

## 🔍 Access

```cpp
cout << mp[1];
```

Output

```text
One
```

---

## ✏️ Update

```cpp
mp[1] = "Hello";
```

Old value is replaced.

---

## 🔎 Search

```cpp
if(mp.find(5) != mp.end())
{
    cout << "Found";
}
```

---

## ❌ Delete

```cpp
mp.erase(5);
```

Deletes key `5`.

---

## 📏 Size

```cpp
mp.size();
```

---

## 📭 Empty

```cpp
mp.empty();
```

---

## 🔁 Traversing a Map

```cpp
for(auto it : mp)
{
    cout << it.first << " ";
    cout << it.second << endl;
}
```

Example

```cpp
map<int,string> mp;

mp[2] = "B";
mp[1] = "A";
mp[3] = "C";
```

Output

```text
1 A
2 B
3 C
```

Notice that the keys are automatically visited in sorted order.

---

# Important Members

| Member   | Meaning |
| -------- | ------- |
| `first`  | Key     |
| `second` | Value   |

Example

```cpp
for(auto x : mp)
{
    cout << x.first;
    cout << x.second;
}
```

---

# Nested Map

Syntax

```cpp
map<int, map<int,int>> mp;
```

Visualization

```text
Column

↓

Row

↓

Value
```

Example

```cpp
mp[0][2] = 15;
```

Meaning

```text
Column = 0

↓

Row = 2

↓

15
```

---

# Multiset

A **multiset** stores values in **sorted order** and **allows duplicates**.

```cpp
multiset<int> st;

st.insert(6);
st.insert(3);
st.insert(5);
st.insert(3);
```

Stored as

```text
3
3
5
6
```

---

# Map + Multiset

Used in **Vertical Order Traversal**.

```cpp
map<int, map<int, multiset<int>>> mp;
```

Meaning

```text
Column

↓

Row

↓

Sorted Values
```

Insertion

```cpp
mp[column][row].insert(node->val);
```

Example

```cpp
mp[0][2].insert(15);
mp[0][2].insert(5);
```

Stored as

```text
Column 0

↓

Row 2

↓

5
15
```

No manual sorting required because `multiset` keeps values ordered.

---

# Map vs Unordered Map

| Feature        | map            | unordered_map |
| -------------- | -------------- | ------------- |
| Order          | Sorted         | Random        |
| Implementation | Red-Black Tree | Hash Table    |
| Search         | O(log n)       | Average O(1)  |
| Insert         | O(log n)       | Average O(1)  |
| Delete         | O(log n)       | Average O(1)  |
| Duplicate Keys | ❌              | ❌             |

Use:

* **map** → when sorted keys are required.
* **unordered_map** → when ordering doesn't matter and average-case speed is preferred.

---

# Common DSA Uses

* Frequency Counting
* Coordinate Compression
* Vertical Order Traversal
* Top View / Bottom View
* Interval Problems
* Sweep Line Algorithms
* Graphs
* Dynamic Programming Memoization

---

# Complexity

| Operation | Time         |
| --------- | ------------ |
| Insert    | **O(log n)** |
| Search    | **O(log n)** |
| Delete    | **O(log n)** |
| Traverse  | **O(n)**     |

---

# 🎯 Key Takeaways

* Stores **Key → Value** pairs.
* Keys are **always sorted**.
* Keys must be **unique**.
* `mp[key]` automatically creates the key if it doesn't exist.
* Nested maps are useful for **2D indexing** (e.g., Column → Row).
* `multiset` is often combined with `map` when sorted duplicate values are required.
