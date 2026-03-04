# 🪟 Understanding Sliding Window

The Sliding Window pattern is used to transform $O(n^2)$ or $O(n^3)$ problems into $O(n)$ time complexity. It is primarily used for **contiguous** sequences (arrays or strings).

---

## ❓ When to Use?
You should consider Sliding Window if the problem meets these criteria:
1. **Linear Data Structure:** Arrays, Strings, or Linked Lists.
2. **Contiguous:** You are looking for a subarray or substring (elements next to each other).
3. **Optimizable:** You are looking for a Min, Max, Longest, Shortest, or Contains property.



---

## 🛠️ Fixed vs. Variable Window

| Type | When to Use? | Example Problem |
| :--- | :--- | :--- |
| **Fixed Size** | Window size $K$ is constant. | Max Sum of Subarray of size $K$. |
| **Variable Size** | Window size changes based on a condition. | Smallest Subarray with Sum $\ge S$. |

---

## ⚖️ Why Use Sliding Window? (Vs. Others)

### 1. Sliding Window vs. Brute Force
- **Brute Force:** Recomputes everything from scratch for every starting position ($O(N \cdot K)$).
- **Sliding Window:** Reuses the result of the previous window and updates it in $O(1)$ by adding the "new" element and removing the "old" one. Total: $O(N)$.

### 2. Sliding Window vs. Two Pointers
- **Two Pointers:** Usually used to search for pairs (e.g., `TwoSum` on a sorted array). The pointers often move toward each other.
- **Sliding Window:** The two pointers represent the **bounds** of a subarray and usually move in the same direction.

### 3. Sliding Window vs. Prefix Sum
- **Prefix Sum:** Better if you need to answer **multiple queries** about different subarray sums.
- **Sliding Window:** Better for **finding** a specific subarray that meets a criteria in a single pass.

---

## 📝 The Template (Variable Window)

Most medium/hard problems follow this structure:

```cpp
int left = 0, right = 0;
while (right < n) {
    // 1. Expand the window by including arr[right]
    window.add(arr[right]);

    // 2. While the condition is broken/met, shrink from the left
    while (condition_met) {
        // Update your result (min/max length)
        result = update(result);
        
        // Remove arr[left] and increment left
        window.remove(arr[left]);
        left++;
    }
    
    // 3. Move right pointer forward
    right++;
}
