---
hide:
  - toc
---

# 242. Valid Anagram

*Edited by Charles*

**Easy · Python · HashTable**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/valid-anagram/description/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        Main idea: make two sets of  counts of each character in s and t. Compare these two sets if they are the same. Remember the one-liner.

    Original study notes follow, including intermediate snippets and earlier attempts.

    1. Main idea: make two sets of  counts of each character in s and t. Compare these two sets if they are the same.
    2. Remember the one-liner.
    3. Remember the string’s built-in method count(), s.count(char).

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 2 |
    | Mastery | ❚❚ |
    | The Latest Attempt | January 5, 2026 |

## Python solution

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
		    # set() is needed for the time complexity
        s_set = {char: s.count(char) for char in set(s)}
        t_set = {char: t.count(char) for char in set(t)}

        if s_set != t_set:
            return False

        return True
```

---

[Back to Day 5 Hash Table I](index.md) · [All Algorithm notes](../index.md)
