---
hide:
  - toc
---

# 455. Assign Cookies

*Edited by Charles*

**Easy · Python · Greedy**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/assign-cookies/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Start feeding from the child with the largest greed factor. Thus, sort g and s first. The logic is if the largest cookie cannot meet the current child’s greed. Iterate backwards to the child with the second largest greed until the greed is fulfilled.

## Python solution

```python
class Solution:
    def findContentChildren(self, g: List[int], s: List[int]) -> int:
        g.sort()
        s.sort()

        result = 0

        child, cookie = len(g) - 1, len(s) - 1

        while child >=0 and cookie >= 0:
            if s[cookie] >= g[child]:
                result += 1
                child -= 1
                cookie -= 1
            else:
                child -= 1

        return result
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. Start feeding from the child with the largest greed factor. Thus, sort g and s first.
    2. The logic is if the largest cookie cannot meet the current child’s greed. Iterate backwards to the child with the second largest greed until the greed is fulfilled.
    3. The terminal condition of the pointers to children and cookies are both of them have to be non-negative.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 26, 2026 |

---

[Back to Day 23 Greedy Algorithm I](index.md) · [All Algorithm notes](../index.md)
