# 402. Remove K Digits

| Topic | Difficulty | Pattern                             |
| ----- | ---------- | ----------------------------------- |
| Stack | Medium     | Greedy + Monotonic Increasing Stack |

---

## 📌 Problem Description

Given a numeric string `num` and an integer `k`, remove exactly `k` digits so that the resulting number is the **smallest possible**.

Return the final number as a string.

---

## 🧠 Intuition

To obtain the smallest possible number, the **leftmost digits should be as small as possible**.

If a smaller digit appears after a larger one, it is beneficial to remove the larger digit.

Example:

```text
1432219

Read 4
Read 3

43 > 34

Remove 4
```

This greedy decision always helps produce the smallest number.

---

## 🎯 Pattern Recognition

Keywords:

* Greedy
* Monotonic Increasing Stack
* Remove Larger Previous Digit
* Smallest Lexicographical Number

---

## ⚙️ Approach

Traverse every digit.

### While:

* Stack is not empty.
* Current digit is smaller than the stack top.
* We still have removals (`k > 0`).

Pop the stack.

Finally push the current digit.

---

### Remaining Removals

If digits were already increasing:

```text
123456
```

No digit gets removed during traversal.

Still remove the last `k` digits.

---

### Remove Leading Zeros

Example:

```text
0200
```

becomes

```text
200
```

---

### Empty String

If every digit gets removed:

```text
""
```

Return:

```text
0
```

---

## 🧪 Dry Run

Input

```text
num = "1432219"
k = 3
```

Process

```text
1 → push

14

3 arrives

4 > 3

Pop 4

Stack

13

2 arrives

3 > 2

Pop 3

Stack

12

2 arrives

Stack

122

1 arrives

2 > 1

Pop 2

Stack

121

k = 0

Push remaining digits
```

Result

```text
1219
```

---

## 📚 Stack Visualization

```text
Read 1

1
```

```text
Read 4

Top
4
1
```

```text
Read 3

4 > 3

Pop

Top
1

Push 3

Top
3
1
```

---

## ⚠️ Common Mistakes

### 1. Using `if` Instead of `while`

Wrong:

```cpp
if(...)
```

Correct:

```cpp
while(...)
```

Multiple larger digits may need to be removed.

---

### 2. Using `>=`

Wrong:

```cpp
st.top() >= digit
```

Correct:

```cpp
st.top() > digit
```

Equal digits should not be removed.

---

### 3. Forgetting Remaining `k`

Example:

```text
12345
```

Nothing gets removed.

Remove the last `k` digits after traversal.

---

### 4. Forgetting to Reverse

Stack produces digits in reverse order.

Reverse the final string.

---

### 5. Forgetting Leading Zeros

Example:

```text
0200
```

Output should be

```text
200
```

---

### 6. Returning Empty String

If every digit is removed:

Return

```text
0
```

---

## 🔁 Similar Problems

* Largest Rectangle in Histogram
* Monotonic Stack
* Remove Duplicate Letters
* Monotonic Queue

---

## 🎯 Key Takeaways

* Always remove a previous larger digit when possible.
* Stack remains monotonically increasing.
* Greedy choice guarantees the smallest number.
* Remove remaining digits from the end if needed.
* Remove leading zeros before returning.

---

## 💻 Implementation (C++)

```cpp
class Solution {
public:
    string removeKdigits(string num, int k) {

        stack<char> st;

        for(char digit : num){

            while(!st.empty() &&
                  st.top() > digit &&
                  k > 0){

                st.pop();
                k--;
            }

            st.push(digit);
        }

        while(k > 0){
            st.pop();
            k--;
        }

        string ans;

        while(!st.empty()){
            ans += st.top();
            st.pop();
        }

        reverse(ans.begin(), ans.end());

        int i = 0;

        while(i < ans.size() && ans[i] == '0'){
            i++;
        }

        ans = ans.substr(i);

        if(ans.empty())
            return "0";

        return ans;
    }
};
```

---

## ⏱️ Complexity Analysis

### Time Complexity

```text
O(n)
```

Each digit is pushed and popped at most once.

### Space Complexity

```text
O(n)
```

For the stack and output string.
