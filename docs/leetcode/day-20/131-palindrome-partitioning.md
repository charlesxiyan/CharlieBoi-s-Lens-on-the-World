---
hide:
  - toc
---

# 131. Palindrome Partitioning

*Edited by Charles*

**Medium · Python · Backtrack**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/palindrome-partitioning/description/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        Recall [77. Combinations](../day-19/77-combinations.md) for the core idea of backtracking. The only difference is in the for loop, we need to check if the new substring is a palindrome.

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. Recall [77. Combinations](../day-19/77-combinations.md) for the core idea of backtracking.
    2. The only difference is in the for loop, we need to check if the new substring is a palindrome.
    3. Take ‘aab’ as an example.
        1. startIndex = 0, i = startIndex = 0, the new substring is s[0: 1] = ‘a’, palindrome check, Tick!
        2. backtrack recurrence but startIndex = i + 1 = 1 (the beginning of the next substring). Within the next recurrence, the new substring is s[1: 2] = ‘a’.
        3. ……

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 22, 2026 |

## Python solution

```python
class Solution:
    def partition(self, s: str) -> List[List[str]]:
        result = []
        self.backtrack(s, 0, [], result)
        return result

    def backtrack(self, s, startIndex, path, result):
        if startIndex >= len(s):
            result.append(path[:])
            return

        for i in range(startIndex, len(s)):
            # Check Palindrome
            if s[startIndex: i + 1] == s[startIndex: i + 1][::-1]:
                path.append(s[startIndex: i + 1])
                self.backtrack(s, i + 1, path, result)
                path.pop()
```

---

[Back to Day 20 Backtrack II](index.md) · [All Algorithm notes](../index.md)
