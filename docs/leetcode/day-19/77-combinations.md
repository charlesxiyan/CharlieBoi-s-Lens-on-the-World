---
hide:
  - toc
---

# 77. Combinations

*Edited by Charles*

**Medium · Python · Backtrack**

[Original problem](https://leetcode.com/problems/combinations/description/)

!!! tip "Core idea"

    Recall [113. Path Sum II](../day-14/113-path-sum-ii.md). The reason why we have result.append(path[:]) is that path[:] works the same as list() in [113. Path Sum II](../day-14/113-path-sum-ii.md) so that path.pop() later would not influence the lists that have already been appended in result. Backtrack solution: 3 Steps

## Python solution

```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        result = []
        self.backtrack(n, k, 1, [], result)
        return result

    def backtrack(self, n, k, startIndex, path, result):
        if len(path) == k:
            result.append(path[:])
            return
				# Pruning: n - i + 1 >= k - len(path)
				# there are n - (i - 1) numbers as options to choose; now we have k - len(path) numbers left to be filled.
				# We need to make sure n - (i - 1) >= k - len(path)
        for i in range(startIndex, n + 2 - (k - len(path))):
            path.append(i)
            self.backtrack(n, k, i + 1, path, result)
            path.pop()
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. Recall [113. Path Sum II](../day-14/113-path-sum-ii.md). The reason why we have result.append(path[:]) is that path[:] works the same as list() in [113. Path Sum II](../day-14/113-path-sum-ii.md) so that path.pop() later would not influence the lists that have already been appended in result.
    2. Backtrack solution: 3 Steps
        1. determine input arguments and the return value of the recursive function.
        2. determine the terminal condition
        3. determine the logic of the recurrence
    3. Pruning:
        1. consider n = 4, k = 4.
        2. The numbers to be chosen are 1, 2, 3, 4. As the startIndex starts from 1, the recurrence within this iteration (startIndex = 1) will return one valid path.
        3. When startIndex iterates to 2 or greater values, they won’t return valid paths, since they even cannot make the len(path) = 4, e.g. [2, 3, 4].
        4. Thus, the recurrence within the iteration should determine the upper bound of the iteration, n + 1 - (k - len(path)) + 1 = n + 2 - (k - len(path))
        5. The upper boundary of i: i <= n + 1 - (k - len(path)), i.e. in n + 2 - (k - len(path)) range.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 21, 2026 |

---

[Back to Day 19 Backtrack I](index.md) · [All LeetCode notes](../index.md)
