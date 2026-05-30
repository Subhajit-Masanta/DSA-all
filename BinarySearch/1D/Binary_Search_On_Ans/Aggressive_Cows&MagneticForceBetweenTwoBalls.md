# 🔍 Aggressive Cows and Magnetic Force Between Two Balls

| Topic | Difficulty | Pattern |
|---|---|---|
| Binary Search | Medium | Binary Search on Answers |

## 📌 Problem

You are given:

```cpp
stalls[]
```

representing stall positions and:

```cpp
k
```

aggressive cows.

Place all cows such that:

```cpp
minimum distance between any two cows
```

is as large as possible.

Return:
# maximum possible minimum distance

---

## 🧠 Core Insight

We are NOT searching stall positions.

We are searching:
# distance

---

## Example

```cpp
stalls = [1,2,4,8,9]
k = 3
```

Place cows:

```cpp
1 _ 4 8 _
```

Distances:

```cpp
4 - 1 = 3
8 - 4 = 4
```

Minimum distance:

```cpp
min(3,4) = 3
```

Answer:

```cpp
3
```

---

# 🔥 Why Binary Search Works

Suppose:

```cpp
distance = 3
```

is possible.

Then:

```cpp
distance = 2
distance = 1
```

are also possible.

Pattern:

```cpp
TTTTFFFF
```

or

```cpp
possible → possible → possible → impossible
```

We want:
# maximum valid distance

---

# 🧠 Search Space

After sorting:

```cpp
low = 1
```

Minimum possible distance.

---

Maximum possible distance:

```cpp
lastStall - firstStall
```

So:

```cpp
high = stalls[n-1] - stalls[0]
```

---

# ⚙️ Greedy Check

For a given distance:

```cpp
mid
```

Place first cow at:

```cpp
stalls[0]
```

Then scan the array.

Whenever:

```cpp
stalls[i] - lastPlaced >= mid
```

place another cow.

Count total cows placed.

---

## If

```cpp
placed >= k
```

distance works.

Try bigger distance:

```cpp
go RIGHT
```

---

## Else

distance too large.

Try smaller distance:

```cpp
go LEFT
```

---

# 🧪 Example

```cpp
stalls = [1,2,4,8,9]
k = 3
distance = 3
```

Place:

```cpp
Cow 1 → 1
```

Next valid:

```cpp
4 - 1 = 3
```

Place:

```cpp
Cow 2 → 4
```

Next valid:

```cpp
8 - 4 = 4
```

Place:

```cpp
Cow 3 → 8
```

Placed:

```cpp
3 cows
```

Success.

---

# ⚙️ Algorithm

1. Sort stalls.
2. Search distance from:

```cpp
1 → stalls[n-1] - stalls[0]
```

3. Binary search on distance.
4. Greedily check if all cows can be placed.
5. If possible:
   ```cpp
   store answer
   go RIGHT
   ```
6. Otherwise:
   ```cpp
   go LEFT
   ```

---

# ⚠️ Common Mistakes

- Forgetting to sort stalls
- Binary searching stall positions instead of distance
- Updating `high` when distance works
- Using adjacent stall distance instead of distance from last placed cow

---

# 💻 Code

```cpp
class Solution {
public:

    bool works(vector<int>& arr, int mid, int k) {

        int placed = 0;
        int last = 0;

        for(int i = 0; i < arr.size(); i++) {

            if(i == 0) {

                placed++;
                last = arr[i];
            }

            if(arr[i] - last >= mid) {

                placed++;
                last = arr[i];
            }
        }

        return placed >= k;
    }

    int aggressiveCows(vector<int>& stalls, int k) {

        sort(stalls.begin(), stalls.end());

        int low = 1;

        int high = stalls[stalls.size() - 1] - stalls[0];

        int ans = 0;

        while(low <= high) {

            int mid = low + (high - low) / 2;

            if(works(stalls, mid, k)) {

                ans = mid;

                low = mid + 1;
            }

            else {

                high = mid - 1;
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
| Sorting | O(n log n) |
| Binary Search | O(n log(range)) |
| Space | O(1) |

---

# 📓 Revision Notes

### Pattern

Binary Search on Answers

### Search Space

```cpp
1 → maxDistance
```

### Validation

Greedy placement

### If Distance Works

```cpp
go RIGHT
```

### If Distance Fails

```cpp
go LEFT
```

### One-liner

Binary search the largest minimum distance that allows placing all cows.
