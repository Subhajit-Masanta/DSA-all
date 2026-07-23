# C++ Tuple Cheat Sheet (DSA)

> A compact guide to everything you need about `tuple` for DSA and Competitive Programming.

---

# What is a Tuple?

A `pair` stores **2 values**.

```cpp
pair<int,int> p;
```

A `tuple` stores **3 or more values** (even different data types).

```cpp
tuple<int,int,int> t;
```

Example

```cpp
tuple<int,string,double> student;

student = {101,"Rahul",8.9};
```

---

# Header File

```cpp
#include <tuple>
```

(Already included in `<bits/stdc++.h>`)

---

# Why use Tuple?

Whenever you need to store more than 2 values together.

Examples

- Dijkstra on Grid
- BFS on Grid
- A* Search
- States `(node, mask, cost)`
- `(distance,row,col)`
- `(cost,node,parent)`
- `(time,node,stops)`

---

# Creating Tuple

### Method 1

```cpp
tuple<int,int,int> t;
```

---

### Method 2

```cpp
tuple<int,int,int> t(2,5,7);
```

---

### Method 3 (Recommended)

```cpp
tuple<int,int,int> t = {2,5,7};
```

---

### Method 4

```cpp
auto t = make_tuple(2,5,7);
```

---

# Accessing Elements

Suppose

```cpp
tuple<int,int,int> t = {10,20,30};
```

---

## Method 1 (Most Common)

Using `get<>`

```cpp
cout << get<0>(t);
```

Output

```text
10
```

---

```cpp
cout << get<1>(t);
```

Output

```text
20
```

---

```cpp
cout << get<2>(t);
```

Output

```text
30
```

---

Remember

```text
Tuple indexing starts from 0.
```

---

## Method 2 (Best in Modern C++)

Structured Binding ⭐⭐⭐⭐⭐

```cpp
auto [a,b,c] = t;
```

Now

```cpp
cout << a;
cout << b;
cout << c;
```

Output

```text
10

20

30
```

This is the method used most often in DSA.

---

## Method 3

Using `tie()`

```cpp
int a,b,c;

tie(a,b,c) = t;
```

Now

```cpp
a = 10

b = 20

c = 30
```

---

# Modify Elements

There is **no**

```cpp
t.first
```

or

```cpp
t.second
```

Instead

```cpp
get<0>(t)=100;
```

Example

```cpp
tuple<int,int,int> t={1,2,3};

get<1>(t)=50;
```

Now

```text
(1,50,3)
```

---

# Size of Tuple

```cpp
tuple<int,int,int,int>
```

contains

```text
4 elements
```

Compile-time

```cpp
tuple_size<decltype(t)>::value
```

Rarely used in DSA.

---

# Compare Tuples

Tuples compare **lexicographically**.

Example

```cpp
tuple<int,int,int> a={1,2,3};

tuple<int,int,int> b={1,5,0};
```

Comparison

```cpp
a<b
```

Result

```text
true
```

Why?

Compare one by one.

```text
1 == 1

↓

2 < 5

↓

Done
```

Third value isn't checked.

Exactly like `pair`.

---

# Swap Tuples

```cpp
swap(t1,t2);
```

---

# Tuple inside Vector

```cpp
vector<tuple<int,int,int>> v;
```

Insert

```cpp
v.push_back({2,4,6});
```

Loop

```cpp
for(auto [a,b,c]:v)
{
    cout<<a<<" "<<b<<" "<<c;
}
```

---

# Tuple inside Queue

```cpp
queue<tuple<int,int,int>> q;
```

Push

```cpp
q.push({1,2,3});
```

Access

```cpp
auto [a,b,c]=q.front();
```

---

# Tuple inside Priority Queue ⭐⭐⭐⭐⭐

Most important for Graphs.

Min Heap

```cpp
priority_queue<
tuple<int,int,int>,
vector<tuple<int,int,int>>,
greater<tuple<int,int,int>>
> pq;
```

Store

```text
(Effort,Row,Col)
```

Push

```cpp
pq.push({5,2,3});
```

Access

```cpp
auto [effort,row,col]=pq.top();

pq.pop();
```

---

# Tuple vs Pair

| Pair | Tuple |
|------|------|
| Stores 2 values | Stores any number of values |
| `.first` | `get<0>()` |
| `.second` | `get<1>()` |
| Cannot store 3 values | Can store 3,4,5... values |
| Lexicographical comparison | Lexicographical comparison |

---

# Pair vs Tuple Example

### Pair

```cpp
pair<int,int> p={4,7};

cout<<p.first;
cout<<p.second;
```

---

### Tuple

```cpp
tuple<int,int,int> t={4,7,9};

cout<<get<0>(t);

cout<<get<1>(t);

cout<<get<2>(t);
```

---

# Common DSA Examples

## Grid Dijkstra

```cpp
priority_queue<
tuple<int,int,int>,
vector<tuple<int,int,int>>,
greater<tuple<int,int,int>>
> pq;

pq.push({0,0,0});
```

Meaning

```text
(Effort,Row,Col)
```

---

## BFS State

```cpp
queue<tuple<int,int,int>> q;

q.push({row,col,steps});
```

Meaning

```text
(Row,Col,Steps)
```

---

## Graph State

```cpp
tuple<int,int,int>

↓

(Cost,Node,Parent)
```

---

## Flight Problem

```cpp
tuple<int,int,int>

↓

(Cost,Node,Stops)
```

---

# Common Mistakes

## ❌ Using `.first`

Wrong

```cpp
t.first
```

Correct

```cpp
get<0>(t)
```

or

```cpp
auto [a,b,c]=t;
```

---

## ❌ Forgetting `greater<>`

Default Priority Queue

```cpp
priority_queue<tuple<int,int,int>>
```

creates a

```text
Max Heap
```

Use

```cpp
greater<tuple<int,int,int>>
```

for a Min Heap.

---

## ❌ Wrong Index

```cpp
get<1>()
```

means

```text
Second Element
```

NOT first.

---

# Cheat Sheet ⭐

### Create

```cpp
tuple<int,int,int> t={1,2,3};
```

---

### Access

```cpp
get<0>(t);

get<1>(t);

get<2>(t);
```

---

### Structured Binding

```cpp
auto [a,b,c]=t;
```

---

### Modify

```cpp
get<2>(t)=100;
```

---

### Vector

```cpp
vector<tuple<int,int,int>> v;
```

---

### Queue

```cpp
queue<tuple<int,int,int>> q;
```

---

### Priority Queue

```cpp
priority_queue<
tuple<int,int,int>,
vector<tuple<int,int,int>>,
greater<tuple<int,int,int>>
> pq;
```

---

# Key Takeaways

- Use `pair` when storing **2 values**.
- Use `tuple` when storing **3 or more values**.
- Access using `get<index>()` or, preferably, **structured binding** (`auto [a,b,c] = t;`).
- `tuple` supports lexicographical comparison automatically, making it work directly with `sort()` and `priority_queue`.
- In DSA, `tuple` is most commonly used in graph/grid problems to store states like `(cost, row, col)` or `(cost, node, stops)`.
