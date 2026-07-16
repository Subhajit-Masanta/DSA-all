# Alien Dictionary (GFG)

| Topic | Difficulty | Pattern |
|--------|------------|---------|
| Graph | Hard | Graph Construction + Topological Sort |

---

# 📌 Problem

You are given a dictionary of words that is already sorted according to an **alien language**.

The alphabet is still

```text
a ... z
```

but the **order of letters is unknown**.

Your task is to determine one valid order of the letters.

If no valid ordering exists, return

```text
""
```

---

# Key Observation

The graph is **NOT given**.

We must build it.

Each

```text
Character = Node
```

The relation between characters becomes

```text
Directed Edge
```

---

# How to Build the Graph

Compare only

```text
words[i]

words[i+1]
```

because the dictionary is already sorted.

---

## Compare Character by Character

Example

```text
abcd

abca
```

Compare

```text
a == a

b == b

c == c

d != a
```

The **first different character** tells us

```text
d comes before a
```

Create edge

```text
d → a
```

Then

```text
Break
```

Never compare remaining characters.

---

## Another Example

```text
cab

cad
```

Compare

```text
c == c

a == a

b != d
```

Create

```text
b → d
```

Break.

---

# Why Only First Difference?

Dictionary order is decided by the first unequal character.

Example

```text
apple

ball
```

We already know

```text
a < b
```

No need to compare

```text
p

a
```

later.

Same idea here.

---

# Invalid Case

Example

```text
abcd

abc
```

All compared characters are equal.

But

```text
abcd
```

appears before

```text
abc
```

Impossible.

Return

```text
""
```

---

# Graph Construction

```text
For every adjacent pair

↓

Compare characters

↓

First different character

↓

Add edge

↓

Break

↓

No difference found ?

↓

If first word is longer

↓

Return ""
```

---

# Algorithm

```text
Build Graph

↓

Compute Indegree

↓

Push all indegree = 0 letters

↓

Kahn's Algorithm

↓

Build Answer String

↓

If every unique letter is processed

↓

Return Answer

Else

↓

Return ""
```

---

# Code

```cpp
class Solution {
public:
    string findOrder(vector<string> &words) {

        vector<vector<int>> adj(26);
        vector<int> indegree(26,0);
        vector<int> present(26,0);

        // Mark existing characters
        for(string word : words)
        {
            for(char ch : word)
                present[ch-'a'] = 1;
        }

        // Build Graph
        for(int i=0;i<words.size()-1;i++)
        {
            string word1 = words[i];
            string word2 = words[i+1];

            int len = min(word1.size(), word2.size());

            bool found = false;

            for(int j=0;j<len;j++)
            {
                if(word1[j] != word2[j])
                {
                    adj[word1[j]-'a'].push_back(word2[j]-'a');
                    indegree[word2[j]-'a']++;
                    found = true;
                    break;
                }
            }

            if(!found && word1.size() > word2.size())
                return "";
        }

        queue<int> q;

        for(int i=0;i<26;i++)
        {
            if(present[i] && indegree[i]==0)
                q.push(i);
        }

        string ans="";

        while(!q.empty())
        {
            int node=q.front();
            q.pop();

            ans += char(node+'a');

            for(int neigh:adj[node])
            {
                indegree[neigh]--;

                if(indegree[neigh]==0)
                    q.push(neigh);
            }
        }

        int totalChars=0;

        for(int i=0;i<26;i++)
        {
            if(present[i])
                totalChars++;
        }

        if(ans.size()!=totalChars)
            return "";

        return ans;
    }
};
```

---

# Dry Run

Input

```text
baa

abcd

abca

cab

cad
```

Compare

```text
baa

abcd
```

Edge

```text
b → a
```

---

Compare

```text
abcd

abca
```

Edge

```text
d → a
```

---

Compare

```text
abca

cab
```

Edge

```text
a → c
```

---

Compare

```text
cab

cad
```

Edge

```text
b → d
```

Graph

```text
b

↓

d

↓

a

↓

c
```

Topological Order

```text
bdac
```

---

# Why Count Total Characters?

Exactly the same idea as

```cpp
processed == numCourses
```

in Course Schedule.

Here,

```cpp
ans.size() == totalChars
```

means

```text
Did Topological Sort process every unique letter?
```

If

```text
No
```

then a cycle exists.

Return

```text
""
```

---

# Common Mistakes

## ❌ Comparing every pair of words

Only compare

```text
words[i]

words[i+1]
```

---

## ❌ Continuing after first difference

Wrong.

After first unequal character

```text
Create Edge

↓

Break
```

---

## ❌ Invalid Prefix Check

Wrong

```cpp
if(word1.size()>word2.size())
```

Correct

```cpp
No different character found

AND

word1 is longer
```

Then return

```text
""
```

---

## ❌ Comparing with 26

Not every letter appears.

Count only the letters that actually exist.

---

# Complexity

| Complexity | Value |
|------------|-------|
| Time | **O(N × L + 26 + E)** |
| Space | **O(26 + E)** |

Where

- **N** = number of words
- **L** = average word length

---

# Recognition Pattern ⭐⭐⭐⭐⭐

Whenever you see

```text
Dictionary

Unknown Alphabet

Sorted Words

Character Ordering
```

Think

```text
Characters = Nodes

↓

Compare Adjacent Words

↓

First Different Character = Edge

↓

Topological Sort
```

---

# Key Takeaways

- Graph is **constructed**, not given.
- Compare only adjacent words.
- The **first different character** gives one directed edge.
- Ignore remaining characters after the first difference.
- Handle the invalid prefix case.
- Run Kahn's Algorithm.
- If all unique letters are processed, return the order; otherwise, return `""`.
