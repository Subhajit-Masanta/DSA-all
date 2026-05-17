# 🔍 69. Sqrt(x)

| Topic | Difficulty | Pattern |
| :--- | :--- | :--- |
| Binary Search | Easy | Binary Search on Answers |

---

# 📌 Problem Description

Given a non-negative integer `x`, return:
```cpp
floor(sqrt(x))
```

Meaning:
- Return the square root rounded DOWN
- Do NOT use built-in functions like:
```cpp
sqrt()
pow()
```

---

# 🧠 Core Insight

This is NOT:
> Binary Search on array

This is:
# ✅ Binary Search on Answer Space

We are searching for:
```cpp
largest integer such that:
mid * mid <= x
```

---

# 🔥 Why Binary Search Works

For:
```cpp
x = 8
```

Possible answers:
```cpp
0 1 2 3 4 5 6 7 8
```

Now check:
```cpp
mid * mid <= x
```

| mid | mid² | valid? |
|---|---|---|
| 0 | 0 | ✅ |
| 1 | 1 | ✅ |
| 2 | 4 | ✅ |
| 3 | 9 | ❌ |
| 4 | 16 | ❌ |

Pattern:
``` id="5k26ty"
TRUE TRUE TRUE FALSE FALSE
```

This monotonic behavior allows Binary Search.

---

# 🧠 Mental Model

We are finding:
# ✅ LAST TRUE

Meaning:
```cpp
largest mid where:
mid * mid <= x
```

---

# ⚠️ IMPORTANT OVERFLOW ISSUE

This:
```cpp
mid * mid
```

can overflow for large values.

Example:
```cpp
46341 * 46341
```

exceeds int range.

So instead of:
```cpp
mid * mid <= x
```

Use:
```cpp
mid <= x / mid
```

This safely avoids overflow. :contentReference[oaicite:0]{index=0}

---

# ⚙️ Binary Search Algorithm

1. Search space:
```cpp
1 → x
```

2. Find mid

3. If:
```cpp
mid <= x / mid
```

Then:
- mid is valid
- maybe bigger answer exists

Move RIGHT:
```cpp
low = mid + 1
```

4. Else:
```cpp
mid > x / mid
```

mid too large.

Move LEFT:
```cpp
high = mid - 1
```

---

# 🧪 Dry Run

Input:
```cpp
x = 8
```

---

## Step 1
```cpp
low = 1
high = 8

mid = 4
```

Check:
```cpp
4 > 8/4
4 > 2
```

Too large.

Move left:
```cpp
high = 3
```

---

## Step 2
```cpp
mid = 2
```

Check:
```cpp
2 <= 8/2
2 <= 4
```

Valid.

Store:
```cpp
ans = 2
```

Try bigger:
```cpp
low = 3
```

---

## Step 3
```cpp
mid = 3
```

Check:
```cpp
3 > 8/3
3 > 2
```

Too large.

Move left:
```cpp
high = 2
```

Loop ends.

Answer:
```cpp
2
```

---

# 🔥 Your Code

```cpp
class Solution {
public:
    int mySqrt(int x) {
        int low =1;
        int high =x;
        int ans;

        while(low<=high){

            int mid=low+(high-low)/2;

            if(mid==x/mid){
                ans=mid;
                break;
            }

            else if(mid>x/mid){
                high=mid-1;
                ans=mid-1;
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

# ✅ What You Did Well

### ✅ Correct Pattern Recognition
You understood:
```cpp
Binary Search on Answers
```

not array searching.

---

### ✅ Overflow Prevention
Excellent use of:
```cpp
mid == x/mid
```

instead of:
```cpp
mid * mid
```

This is interview-level thinking. :contentReference[oaicite:1]{index=1}

---

### ✅ Correct Direction Movement

When:
```cpp
mid > x/mid
```

You moved LEFT.

When:
```cpp
mid < x/mid
```

You moved RIGHT.

Correct.

---

# ⚠️ Problems In Your Code

---

# ❌ Problem 1 — x = 0 Case

Input:
```cpp
x = 0
```

Then:
```cpp
low = 1
high = 0
```

Loop never runs.

`ans` remains uninitialized.

---

# ✅ Fix

```cpp
if(x == 0) return 0;
```

---

# ❌ Problem 2 — Weak Binary Search Thinking

You wrote:
```cpp
ans = mid - 1;
```

inside:
```cpp
mid > x/mid
```

This works.

BUT:
You are guessing answer indirectly.

Cleaner approach:
- Only update answer when condition is VALID

That is stronger interview thinking.

---

# 🔥 Cleaner Version (Recommended)

```cpp
class Solution {
public:
    int mySqrt(int x) {

        if(x == 0)
            return 0;

        int low = 1;
        int high = x;

        int ans = 0;

        while(low <= high) {

            int mid = low + (high - low) / 2;

            if(mid <= x / mid) {

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

# 🧠 Why This Version Is Better

We ONLY update answer when:
```cpp
mid <= x/mid
```

Meaning:
```cpp
mid² <= x
```

valid.

This perfectly matches:
# LAST TRUE pattern

---

# 🔥 Pattern Recognition

This problem teaches:

| Concept | Importance |
|---|---|
| Binary Search on Answers | VERY HIGH |
| LAST TRUE Pattern | VERY HIGH |
| Monotonic Condition | CORE IDEA |
| Overflow Handling | INTERVIEW CRITICAL |

---

# ⚠️ Common Mistakes

- Using:
```cpp
mid * mid
```
without overflow handling

- Forgetting:
```cpp
x = 0
```

- Thinking this is array binary search

- Updating answer incorrectly

---

# ⏱ Complexity

| Type | Complexity |
|---|---|
| Time | O(log x) |
| Space | O(1) |

---

# 📓 Revision Notes

### Pattern:
Binary Search on Answers

### Key Idea:
Find largest integer whose square is <= x

### Monotonic Condition:
```cpp
mid <= x/mid
```

### Binary Search Type:
LAST TRUE

### One-liner:
Search answer space using monotonic validity condition.

---

# 🔥 Important Interview Connection

This problem is foundation for:
- Nth Root
- Koko Eating Bananas
- Minimum Days
- Allocate Books
- Aggressive Cows

If you don't deeply understand THIS,
you will struggle later.

---

# 🧠 Final Mental Model

Binary Search works whenever:
``` id="pklc1f"
TTTTFFFF
```

or
``` id="j1qlcv"
FFFFTTTT
```

exists.

You are NOT searching elements.

You are searching:
# transition point
