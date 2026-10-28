---
hide:
  - toc
---

# 344. Reverse String

*Edited by Charles*

**Easy · Python · String**

[Original problem](https://leetcode.com/problems/reverse-string/)

!!! tip "Core idea"

    The input is a list. Double pointers is the source code idea of built-in function reverse.

## Python solution

```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        # s[:] = s[::-1]
        left, right = 0, len(s) - 1

        while left < right:
            s[left], s[right] = s[right], s[left]
            left += 1
            right -= 1
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. The input is a list.
    2. Double pointers is the source code idea of built-in function reverse.
    3. In Python we can also use [::-1] to reverse.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 6, 2026 |

---

[Back to Day 7 String I](index.md) · [All LeetCode notes](../index.md)
