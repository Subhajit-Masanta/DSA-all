# 🔍 1482_Minimum_Number_of_Days_to_Make_m_Bouquets

| Topic | Difficulty | Pattern |
|---|---|---|
| Binary Search | Medium | Binary Search on Answers |

## 📌 Problem

You are given:

```cpp
bloomDay[i]
```

where:

```cpp
bloomDay[i]
```

represents the day on which the flower blooms.

You need:

```cpp
m bouquets
```

Each bouquet requires:

```cpp
k adjacent flowers
```

Return:
# minimum number of days

required to make at least:

```cpp
m bouquets
```

---

## 🧠 Core Insight

We are NOT searching an array index.

We are searching:
# minimum day

Possible answer range:

```cpp
1 → max(bloomDay)
```

---

# 🔥 Binary Search Question

Suppose:

```cpp
day = mid
```

Ask:

# "If I wait exactly mid days, can I make at least m bouquets?"

---

## If YES

Maybe fewer days also work.

```cpp
go LEFT
```

---

## If NO

Need more flowers to bloom.

```cpp
go RIGHT
```

---

# 🧠 How To Check A Day

A flower is usable if:

```cpp
bloomDay[i] <= day
```

Otherwise:

```cpp
bloomDay[i] > day
```

Flower is unavailable.

---

## Count Consecutive Flowers

Keep:

```cpp
flower = 0
```

For every flower:

---

### If Bloomed

```cpp
flower++
```

---

### If Not Bloomed

Adjacency breaks.

```cpp
flower = 0
```

---

### If

```cpp
flower == k
```

One bouquet formed.

```cpp
bou++
flower = 0
```

Reset because flowers can only be used once.

---

# 🧪 Example

```cpp
bloomDay = [7,7,7,7,12,7,7]
m = 2
k = 3
day = 7
```

Convert:

```cpp
[x x x x _ x x]
```

---

Count:

```cpp
x x x
```

One bouquet.

Reset.

Remaining:

```cpp
x _ x x
```

No second bouquet.

Total:

```cpp
1 bouquet
```

Not enough.

---

## Day = 12

```cpp
[x x x x x x x]
```

Total flowers:

```cpp
7
```

Bouquets:

```cpp
7 / 3 = 2
```

Enough.

Answer:

```cpp
12
```

---

# ⚠️ Important Edge Case

Before Binary Search:

Check:

```cpp
m * k > bloomDay.size()
```

If true:

```cpp
return -1
```

Because enough flowers do not exist.

Example:

```cpp
n = 5
m = 3
k = 2
```

Need:

```cpp
6 flowers
```

Only:

```cpp
5 flowers
```

Impossible.

---

# ⚙️ Algorithm

1. Search range:

```cpp
1 → max(bloomDay)
```

2. Binary search on:

```cpp
day
```

3. Check how many bouquets can be formed.

4. If:

```cpp
bouquets >= m
```

Store answer.

```cpp
go LEFT
```

5. Else:

```cpp
go RIGHT
```

---

# 💻 Code

```cpp
class Solution {
public:

    bool works(vector<int>& arr, int mid, int m, int k) {

        int day = mid;

        int flower = 0;
        int bou = 0;

        for(int i = 0; i < arr.size(); i++) {

            if(arr[i] <= day) {

                flower++;

                if(flower == k) {

                    bou++;

                    flower = 0;
                }
            }

            else {

                flower = 0;
            }
        }

        return bou >= m;
    }

    int findmax(vector<int>& arr) {

        int m = arr[0];

        for(int i = 1; i < arr.size(); i++) {
            m = max(m, arr[i]);
        }

        return m;
    }

    int minDays(vector<int>& bloomDay, int m, int k) {

        if((long long)m * k > bloomDay.size()) {
            return -1;
        }

        int low = 1;

        int high = findmax(bloomDay);

        int ans = -1;

        while(low <= high) {

            int mid = low + (high - low) / 2;

            if(works(bloomDay, mid, m, k)) {

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
| Time | O(n × log(maxBloomDay)) |
| Space | O(1) |

---

# 📓 Revision Notes

### Pattern

Binary Search on Answers

### Search Space

```cpp
1 → max(bloomDay)
```

### Valid Flower

```cpp
bloomDay[i] <= day
```

### Adjacency Break

```cpp
flower = 0
```

### Bouquet Formed

```cpp
flower == k
```

### If Valid

```cpp
bouquets >= m
```

go LEFT

### If Invalid

go RIGHT

### One-liner

Binary search the minimum day on which enough adjacent flowers are available to form all bouquets.
