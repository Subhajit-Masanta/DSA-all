| Topic      | Difficulty | Pattern                      |
| :--------- | :--------- | :--------------------------- |
| Merge Sort | Medium     | Divide & Conquer + Recursion |

---

# 📌 Problem Description

Sort an array using the **Merge Sort** algorithm.

Merge Sort follows:

```text
Divide

↓

Sort Left

↓

Sort Right

↓

Merge
```

It is one of the most important Divide & Conquer algorithms.

---

# 🧠 Core Insight

Instead of sorting the whole array,

```text
[4,1,3,9,7]
```

divide it into smaller parts until every part has only one element.

```text
[4,1,3,9,7]

↓

[4,1,3]   [9,7]

↓

[4,1] [3] [9] [7]

↓

[4] [1] [3] [9] [7]
```

A single element is already sorted.

---

# ⚙️ Algorithm

```text
mergeSort(l,r)

↓

Base Case

↓

Find Mid

↓

Sort Left Half

↓

Sort Right Half

↓

Merge Both Halves
```

---

# 🌳 Recursion Tree

Example

```text
arr = [4,1,3,9,7]
```

```text
                    (0,4)
                   /     \
              (0,2)      (3,4)
              /   \       /   \
          (0,1) (2,2)  (3,3) (4,4)
          /   \
      (0,0) (1,1)
```

Every node represents

```text
mergeSort(l,r)
```

---

# 📚 Call Phase

The recursion only divides.

```text
mergeSort(0,4)

↓

mergeSort(0,2)

↓

mergeSort(0,1)

↓

mergeSort(0,0)

↓

Base Case

return
```

---

# 🔄 Return Phase

Now recursion starts coming back.

```text
mergeSort(0,0)

↓

return

↑

mergeSort(1,1)

↓

return

↑

merge(0,0,1)

↓

[4] [1]

↓

[1,4]
```

---

Next

```text
mergeSort(2,2)

↓

return

↑

merge(0,1,2)

↓

[1,4] [3]

↓

[1,3,4]
```

---

Right subtree

```text
mergeSort(3,3)

↓

return

↑

mergeSort(4,4)

↓

return

↑

merge(3,3,4)

↓

[9] [7]

↓

[7,9]
```

---

Final Merge

```text
merge(0,2,4)

↓

[1,3,4]

+

[7,9]

↓

[1,3,4,7,9]
```

---

# 📚 Complete Call Stack

```text
CALL PHASE

mergeSort(0,4)

↓

mergeSort(0,2)

↓

mergeSort(0,1)

↓

mergeSort(0,0)

↓

return

↑

mergeSort(1,1)

↓

return

↑

merge(0,0,1)

↑

mergeSort(2,2)

↓

return

↑

merge(0,1,2)

↑

mergeSort(3,4)

↓

mergeSort(3,3)

↓

return

↑

mergeSort(4,4)

↓

return

↑

merge(3,3,4)

↑

merge(0,2,4)

↑

Finished
```

---

# 🧠 Merge Logic

Two sorted arrays

```text
Left

1 4

Right

2 5
```

Compare

```text
1 < 2

↓

Take 1

↓

4 > 2

↓

Take 2

↓

4 < 5

↓

Take 4

↓

Take remaining 5
```

Result

```text
1 2 4 5
```

Uses the **Two Pointer** pattern.

---

# ❓Why

```cpp
arr[l+i] = temp[i];
```

and NOT

```cpp
arr[i] = temp[i];
```

Suppose

```text
merge(arr,3,3,4)
```

Current array

```text
Index

0 1 2 3 4

Array

4 1 3 9 7
```

We are only merging

```text
Index

3 4

↓

9 7
```

After merge

```text
temp

Index

0 1

↓

7 9
```

---

## Wrong

```cpp
arr[i]=temp[i];
```

Execution

```text
arr[0]=7

arr[1]=9
```

Array becomes

```text
7 9 3 9 7
```

❌ Wrong.

---

## Correct

```cpp
arr[l+i]=temp[i];
```

Execution

```text
arr[3]=7

arr[4]=9
```

Array becomes

```text
4 1 3 7 9
```

✅ Correct.

---

# 💻 Code (C++)

```cpp
class Solution {
public:

    void merge(vector<int>& arr,int l,int mid,int r)
    {
        vector<int> temp;

        int i=l;
        int j=mid+1;

        while(i<=mid && j<=r)
        {
            if(arr[i]<arr[j])
            {
                temp.push_back(arr[i]);
                i++;
            }
            else
            {
                temp.push_back(arr[j]);
                j++;
            }
        }

        while(i<=mid)
        {
            temp.push_back(arr[i]);
            i++;
        }

        while(j<=r)
        {
            temp.push_back(arr[j]);
            j++;
        }

        for(int i=0;i<temp.size();i++)
        {
            arr[l+i]=temp[i];
        }
    }

    void mergeSort(vector<int>& arr,int l,int r)
    {
        if(l>=r)
            return;

        int mid=l+(r-l)/2;

        mergeSort(arr,l,mid);

        mergeSort(arr,mid+1,r);

        merge(arr,l,mid,r);
    }
};
```

---

# ⏱ Time Complexity

Every level merges all `n` elements.

Number of levels

```text
n

↓

n/2

↓

n/4

↓

...

↓

1
```

Total

```text
O(n log n)
```

---

# 📦 Space Complexity

```text
Temp Array

O(n)

+

Recursion Stack

O(log n)

↓

Overall

O(n)
```

---

# ⚠️ Common Mistakes

❌

```cpp
arr[i]=temp[i];
```

Use

```cpp
arr[l+i]=temp[i];
```

---

❌

```cpp
while(i<mid)
```

Correct

```cpp
while(i<=mid)
```

---

❌ Forgetting to copy remaining elements.

---

# 🔥 Key Insight

```text
CALL PHASE

Divide

↓

Divide

↓

Divide

↓

Base Case

--------------------

RETURN PHASE

Merge Small Arrays

↓

Merge Bigger Arrays

↓

Merge Complete Array
```

---

# 🎯 Interview One-Liner

> **Merge Sort is a Divide & Conquer algorithm: divide during the call phase and merge during the return phase, achieving O(n log n) time complexity.**

---

# 📚 Pattern Recognition

```text
Can the problem be divided into two independent halves?

↓

YES

↓

Solve Left

↓

Solve Right

↓

Combine Results

↓

Divide & Conquer
```
