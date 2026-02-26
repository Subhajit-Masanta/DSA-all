918. Maximum Sum Circular Subarray
Topic	Difficulty	Pattern
Arrays	Medium	Kadane's Algorithm + Circular Logic
📌 Problem Description

Given a circular integer array nums of length n, return the maximum possible sum of a non-empty subarray.

🔄 Circular Property

The end of the array connects back to the beginning.

Each element can be used at most once in a subarray.

🧠 Core Concept — Two-Case Logic

The maximum sum must be in one of two cases:

1️⃣ Normal (Linear) Maximum Subarray

The subarray does not wrap around.

Use Kadane’s Algorithm

Let this be maxSum

2️⃣ Circular (Wrapping) Maximum Subarray

The subarray wraps from end → beginning.

Instead of directly finding it:

Circular Max = Total Sum − Minimum Subarray

Why?

If we remove the minimum subarray (worst middle part)
we are left with the maximum circular segment.

Let:

minSum = minimum subarray (Kadane inverted)

totalSum = sum of entire array

⚙️ Algorithm Steps

Compute totalSum

Run Kadane to get:

maxSum

minSum

Edge case:

If all numbers are negative → return maxSum

Otherwise:

return max(maxSum, totalSum - minSum);
🧠 Final Formula
if (maxSum < 0)
    return maxSum;
else
    return max(maxSum, totalSum - minSum);
🧪 Example 1 — Circular Wins

Input

[5, -3, 5]

Calculation

Total Sum = 7
maxSum = 7
minSum = -3
Circular = 7 - (-3) = 10

Answer

10
🧪 Example 2 — All Negative

Input

[-3, -2, -3]

Calculation

Total Sum = -8
maxSum = -2
minSum = -8
Circular = 0 ❌ (invalid empty subarray)

Since all numbers are negative:

Answer = -2
⏱️ Complexity
Type	Complexity
Time	O(n)
Space	O(1)
⚠️ Common Mistakes

❌ Simulating circular array using modulo

❌ Forgetting totalSum

❌ Using maxSum - minSum

❌ Ignoring all-negative case

❌ Thinking circular means only first + last element

🎯 Interview One-Liner

Compute normal maximum subarray and circular maximum using totalSum − minimum subarray, then return the larger one.

💻 C++ Implementation
class Solution {
public:
    int maxSubarraySumCircular(vector<int>& nums) {
        int totalSum = 0;
        int currentMax = 0;
        int maxSum = nums[0];
        int currentMin = 0;
        int minSum = nums[0];

        for (int x : nums) {
            totalSum += x;

            currentMax = max(x, currentMax + x);
            maxSum = max(maxSum, currentMax);

            currentMin = min(x, currentMin + x);
            minSum = min(minSum, currentMin);
        }

        if (maxSum < 0)
            return maxSum;

        return max(maxSum, totalSum - minSum);
    }
};
