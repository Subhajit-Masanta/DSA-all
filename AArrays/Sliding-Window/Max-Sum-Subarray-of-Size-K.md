# Max Sum Subarray of Size K

| Topic | Difficulty | Pattern |
| :--- | :--- | :--- |
| Arrays | Easy | Sliding Window (Fixed Size) |

---

## 📌 Problem Description
Given an integer array `arr[]` and an integer `k`, return the maximum sum of any contiguous subarray of size `k`.

---

## 🧠 Core Concept: Sliding Window (Fixed Size)

Since the subarray size is fixed at `k`, we don't need to re-sum all elements when moving from one subarray to the next. We simply "slide" the window.



**The Window Update:**
When moving the window one position to the right:
1. **Subtract** the element that is falling out (left side).
2. **Add** the element that is entering (right side).

$$NewSum = OldSum - arr[i - k] + arr[i]$$

---

## ⚙️ The Algorithm

1. **Initialize:** Compute the sum of the first `k` elements (indices $0$ to $k-1$).
2. **Track:** Set `maxSum = currentSum`.
3. **Slide:** Iterate from index `k` to $n-1$:
   - Update `currentSum` by adding the new element and removing the old one.
   - Update `maxSum` if the `currentSum` is higher.
4. **Result:** Return `maxSum`.

---

## 🧪 Example Walkthrough
**Input:** `[1, 4, 2, 10, 23, 3]`, `k = 4`

1. **First Window `[1, 4, 2, 10]`:** Sum = `17`. `maxSum = 17`.
2. **Slide to `[4, 2, 10, 23]`:** $17 - 1 + 23 = 39$. `maxSum = 39`.
3. **Slide to `[2, 10, 23, 3]`:** $39 - 4 + 3 = 38$. `maxSum = 39`.

**Final Answer:** `39`

---

## ⏱️ Complexity Analysis
- **Time Complexity:** $O(n)$ — We visit each element exactly once.
- **Space Complexity:** $O(1)$ — We only store a few integer variables.

---

## ⚠️ Common Mistakes to Avoid
- **Nested Loops:** Using a nested loop to sum $k$ elements at every index results in $O(n \cdot k)$, which is inefficient for large $n$.
- **Indexing Error:** Subtracting `arr[i]` instead of `arr[i-k]`.
- **Initialization:** Forgetting to handle the case where `k` is larger than the array size (though usually constraints prevent this).

---

## 💻 Implementation (C++)

```cpp
class Solution {
public:
    int maxSubarraySum(vector<int>& arr, int k) {
        int n = arr.size();
        if (n < k) return 0;

        long long currentSum = 0;
        
        // 1. Compute initial window
        for (int i = 0; i < k; i++) {
            currentSum += arr[i];
        }

        long long maxSum = currentSum;

        // 2. Slide the window
        for (int i = k; i < n; i++) {
            currentSum += arr[i] - arr[i - k];
            maxSum = max(maxSum, currentSum);
        }

        return (int)maxSum;
    }
};
