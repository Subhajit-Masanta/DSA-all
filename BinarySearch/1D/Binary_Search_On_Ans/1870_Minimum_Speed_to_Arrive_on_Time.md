# 🔍1870_Minimum_Speed_to_Arrive_on_Time

| Topic | Difficulty | Pattern |
|---|---|---|
| Binary Search | Medium | Binary Search on Answers |

## 📌 Problem

You must travel using multiple trains.

```cpp
dist[i]
```

represents the distance of the `i-th` train.

All trains travel at the same speed:
```cpp
speed
```

Return:
# minimum speed

such that total travel time:
```cpp
<= hour
```

---

## ⚠️ Special Rule

Trains can only depart at:
```cpp
integer hours
```

Example:

If train ride takes:
```cpp
1.5 hours
```

Then next train starts at:
```cpp
2.0 hours
```

You must wait:
```cpp
0.5 hour
```

---

## 🧠 Core Insight

We are NOT searching array indexes.

We are searching:
# speed

Possible speeds:
```cpp
1 → 10^7
```

---

# 🔥 Why Binary Search Works

As:
```cpp
speed increases
```

Total travel time:
```cpp
decreases
```

Pattern:

```cpp
FFFFTTTT
```

Where:

```cpp
F = cannot arrive on time
T = can arrive on time
```

We need:
# first valid speed

---

# ⚙️ Time Calculation

For every train except last:

```cpp
ceil(dist[i] / speed)
```

because next train can only leave at an integer hour.

---

## Example

```cpp
dist = 1
speed = 3
```

Time:

```cpp
1/3 = 0.333
```

Next train leaves at:

```cpp
1 hour
```

So:

```cpp
ceil(0.333) = 1
```

---

## Last Train

No waiting after last train.

Use:

```cpp
dist[last] / speed
```

Normal decimal division.

---

# ⚙️ Algorithm

1. Search speed from:

```cpp
1 → 10^7
```

2. Binary search on:

```cpp
speed
```

3. For every `mid`:
   - calculate total travel time

4. If:

```cpp
totalTime <= hour
```

speed works.

Try smaller speed:

```cpp
go LEFT
```

5. Else:

```cpp
speed too slow
```

Need larger speed:

```cpp
go RIGHT
```

---

# 🧪 Example

```cpp
dist = [1,3,2]
hour = 2.7
```

Suppose:

```cpp
speed = 3
```

---

## Train 1

```cpp
1/3 = 0.333
```

Wait until:

```cpp
1 hour
```

Contribution:

```cpp
ceil(0.333) = 1
```

---

## Train 2

```cpp
3/3 = 1
```

Contribution:

```cpp
1
```

Current:

```cpp
2 hours
```

---

## Train 3

```cpp
2/3 = 0.666
```

No waiting.

Total:

```cpp
2.666
```

Which is:

```cpp
<= 2.7
```

So speed 3 works.

---

# ⚠️ Important Observation

If:

```cpp
hour <= n - 1
```

Often impossible.

Because:

```cpp
first n-1 trains alone require at least n-1 hours
```

due to integer departure constraints.

---

# ⚠️ Common Mistakes

- Using floor instead of ceil
- Applying ceil to last train
- Integer division instead of double division
- Binary searching on distances instead of speed
- Forgetting waiting time rule

---

# 💻 Code

```cpp
class Solution {
public:

    bool works(vector<int>& dist, int mid, double hour) {

        int n = dist.size();

        double total = 0.0;

        for(int i = 0; i < n; i++) {

            if(i < n - 1) {

                total += ceil((double)dist[i] / mid);
            }

            else {

                total += (double)dist[i] / mid;
            }
        }

        return total <= hour;
    }

    int minSpeedOnTime(vector<int>& dist, double hour) {

        int low = 1;

        int high = 10000000;

        int ans = -1;

        while(low <= high) {

            int mid = low + (high - low) / 2;

            if(works(dist, mid, hour)) {

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
| Time | O(n × log(10⁷)) |
| Space | O(1) |

---

# 📓 Revision Notes

### Pattern

Binary Search on Answers

### Search Space

```cpp
1 → 10^7
```

### Validation

```cpp
time <= hour
```

### First n-1 Trains

```cpp
ceil(dist[i] / speed)
```

### Last Train

```cpp
dist[i] / speed
```

### If Valid

go LEFT

### If Invalid

go RIGHT

### One-liner

Binary search the minimum speed that allows arrival within the given time.
