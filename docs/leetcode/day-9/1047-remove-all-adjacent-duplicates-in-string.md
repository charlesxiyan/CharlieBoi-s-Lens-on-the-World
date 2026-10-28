---
hide:
  - toc
---

# 1047. Remove All Adjacent Duplicates In String

*Edited by Charles*

**Easy · Python · Stack**

[Original problem](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/)

!!! tip "Core idea"

    This question is quite similar to [20. Valid Parentheses](20-valid-parentheses.md). Iterate the whole string.

## Python solution

```python
class Solution:
    def removeDuplicates(self, s: str) -> str:
        n = len(s)
        stack = [s[0]]

        for i in range(1, n):
            if not stack or s[i] != stack[-1]:
                stack.append(s[i])
            else:
                stack.pop()

        return ''.join(stack)
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. This question is quite similar to [20. Valid Parentheses](20-valid-parentheses.md).
    2. Iterate the whole string.
        1. If the character is not in the top of the stack list or the stack list is empty, append it. For example, “axxazy” will result in empty stack list before the pointer comes through s[4].
        2. Else, cancel the repeated character in the stack list.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 9, 2026 |

---

[Back to Day 9 Stack & Queue I](index.md) · [All LeetCode notes](../index.md)
