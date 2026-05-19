# 🔍 875. Koko Eating Bananas

| Topic | Difficulty | Pattern |
|---|---|---|
| Binary Search | Medium | Binary Search on Answers |

## 📌 Problem

Koko has banana piles:
```cpp
piles[i]
```

She eats:
```cpp
k bananas/hour
```

Goal:
# find MINIMUM k

such that all bananas are finished within:
```cpp
h hours
```

---

## 🧠 Core Insight

We are NOT binary searching on array indexes.

We are binary searching on:
# possible answers

Possible values of:
```cpp
k
```

Range:
```cpp
1 → max(piles)
```

---

# 🔥 Why Binary Search Works

As:
```cpp
k increases
```

hours needed:
```cpp
decrease
```

Example:

| k | Hours Needed |
|---|---|
| 1 | huge |
| 2 | smaller |
| 3 | smaller |
| 4 | valid |
| 5 | valid |

Pattern:
```cpp
FFFFTTTT
```

We want:
# FIRST TRUE

---

# 🧠 Main Binary Search Question

For some:
```cpp
mid = k
```

Ask:
# Can Koko finish within h hours?

---

# ⚙️ Hours Formula

For one pile:
```cpp
ceil(pile / k)
```

Integer form:
```cpp
(pile + k - 1) / k
```

---

## 🧪 Example

```cpp
pile = 11
k = 4
```

Hours:
```cpp
(11 + 4 - 1)/4
= 14/4
= 3
```

Correct:
```cpp
4 + 4 + 3
```

needs:
```cpp
3 hours
```

---

# ⚙️ Algorithm

1. Search range:
```cpp
low = 1
high = maxPile
```

2. Binary Search on:
```cpp
k
```

3. For every `mid`:
- calculate total hours

4. If:
```cpp
totalHours <= h
```

then:
```cpp
k works
```

Try smaller answer:
```cpp
go LEFT
```

5. Else:
```cpp
k too slow
```

Need larger speed:
```cpp
go RIGHT
```

---

# 🧪 Example

```cpp
piles = [3,6,7,11]
h = 8
```

Suppose:
```cpp
k = 4
```

Hours:

| Pile | Hours |
|---|---|
| 3 | 1 |
| 6 | 2 |
| 7 | 2 |
| 11 | 3 |

Total:
```cpp
8
```

Valid.

Try smaller `k`.

---

# ⚠️ Important Overflow Issue

This:
```cpp
(arr[i] + k - 1)
```

can overflow `int`.

Use:
```cpp
long long
```

---

# ⚠️ Common Mistakes

- Using linear search on k
- Forgetting ceiling division
- Using floor division accidentally
- Integer overflow
- Wrong binary search direction

---

# 💻 Code

```cpp
class Solution {
public:

    bool works(vector<int>& arr, int mid, int h) {

        int k = mid;

        long long total = 0;

        for(int i = 0; i < arr.size(); i++) {

            total += ((long long)arr[i] + k - 1) / k;
        }

        return total <= h;
    }

    int findmax(vector<int>& piles) {

        int m = piles[0];

        for(int i = 1; i < piles.size(); i++) {
            m = max(m, piles[i]);
        }

        return m;
    }

    int minEatingSpeed(vector<int>& piles, int h) {

        int low = 1;
        int high = findmax(piles);

        int ans = 0;

        while(low <= high) {

            int mid = low + (high - low) / 2;

            if(works(piles, mid, h)) {

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
| Time | O(n × log(maxPile)) |
| Space | O(1) |

---

# 📓 Revision Notes

### Pattern
Binary Search on Answers

### Search Space
```cpp
1 → maxPile
```

### Main Question
```cpp
Can this k work?
```

### If Valid
go LEFT

### If Invalid
go RIGHT

### Core Idea
Find FIRST valid answer

### One-liner
Binary search on eating speed using monotonic validity.
