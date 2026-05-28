# 🔍 1011_Capacity_To_Ship_Packages_Within_D_Days

| Topic | Difficulty | Pattern |
|---|---|---|
| Binary Search | Medium | Binary Search on Answers |

## 📌 Problem

Given package weights:
```cpp
weights[i]
```

Ship packages within:
```cpp
days
```

Rules:
- packages must remain in given order
- ship has fixed capacity
- one day cannot exceed capacity

Return:
# minimum ship capacity

---

## 🧠 Core Insight

We are NOT binary searching array indexes.

We are binary searching:
# ship capacity

---

# 🔥 Search Space

## Minimum Capacity

Must at least carry:
```cpp
largest package
```

So:
```cpp
low = max(weights)
```

---

## Maximum Capacity

Ship everything in one day:
```cpp
sum(weights)
```

So:
```cpp
high = sum(weights)
```

---

# 🧠 Binary Search Question

For some:
```cpp
mid = capacity
```

Ask:
# Can this capacity ship everything within given days?

---

# ⚙️ Validation Logic

Simulate loading packages.

Keep:
```cpp
currentWeight
```

representing:
```cpp
current day's load
```

---

## If:
```cpp
currentWeight + arr[i] > capacity
```

Then:
# next day starts

```cpp
count++
currentWeight = arr[i]
```

Current package becomes:
```cpp
first package of new day
```

---

## Else:
```cpp
currentWeight += arr[i]
```

Add package normally.

---

# ⚙️ Algorithm

1. Search capacity from:
```cpp
max(weights) → sum(weights)
```

2. Binary search on:
```cpp
capacity
```

3. For every `mid`:
- simulate shipping
- calculate days needed

4. If:
```cpp
daysNeeded <= days
```

capacity works.

Try smaller:
```cpp
go LEFT
```

5. Else:
```cpp
capacity too small
```

Need larger:
```cpp
go RIGHT
```

---

# 🧪 Example

```cpp
weights = [1,2,3,4,5,6,7,8,9,10]
days = 5
```

Suppose:
```cpp
capacity = 15
```

---

## Day 1

```cpp
1+2+3+4+5 = 15
```

Next:
```cpp
6
```

Exceeds capacity.

New day.

---

## Day 2

```cpp
6+7 = 13
```

Next:
```cpp
8
```

Exceeds.

New day.

---

Eventually:
```cpp
5 days needed
```

So:
```cpp
15 works
```

---

# ⚠️ Important Understanding

This problem is:
# Binary Search + Greedy Simulation

We greedily:
```cpp
fill each day as much as possible
```

---

# ⚠️ Common Mistakes

- Forgetting package order matters
- Using wrong search space
- Resetting currentWeight incorrectly
- Confusing days and capacity
- Using linear search instead of binary search

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

    bool works(vector<int>& arr, int mid, int days) {

        int capacity = mid;

        int count = 1;

        int currentWeight = 0;

        for(int i = 0; i < arr.size(); i++) {

            // Exceeds capacity → next day
            if(currentWeight + arr[i] > capacity) {

                count++;

                currentWeight = arr[i];
            }

            // Add package normally
            else {
                currentWeight += arr[i];
            }
        }

        return count <= days;
    }
    
    int shipWithinDays(vector<int>& weights, int days) {

        int low = findmax(weights);

        int high = sum(weights);

        int ans = 0;

        while(low <= high) {

            int mid = low + (high - low) / 2;

            if(works(weights, mid, days)) {

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
| Time | O(n × log(sum(weights))) |
| Space | O(1) |

---

# 📓 Revision Notes

### Pattern
Binary Search on Answers

### Search Space
```cpp
max(weights) → sum(weights)
```

### Main Question
```cpp
Can this capacity work?
```

### Validation
Greedy loading simulation

### If Valid
go LEFT

### If Invalid
go RIGHT

### One-liner
Binary search minimum valid ship capacity using greedy simulation.
