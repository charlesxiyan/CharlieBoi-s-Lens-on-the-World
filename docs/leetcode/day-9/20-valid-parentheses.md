---
hide:
  - toc
---

# 20. Valid Parentheses

*Edited by Charles*

**Easy · Python · Stack**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/valid-parentheses/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    There are three cases to be focused. Iterate the whole string. Once we find a left bracket, we append the corresponding right bracket to the stack list. Once we find a right bracket, we compare this right bracket with the top of the stack (the last added element in the list) to see if they match (case c).

## Python solution

```python
class Solution:
    def isValid(self, s: str) -> bool:
        parentheses = {"(": ")", "[": "]", "{": "}"}

        stack = []
        n = len(s)

        for i in range(n):
            if s[i] in parentheses.keys():
                stack.append(parentheses[s[i]])
            elif not stack or s[i] != stack[-1]:
                return False
            else:
                stack.pop()

        return True if not stack else False
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    1. There are three cases to be focused.
        1. Extra left brackets, say **(**{}
        2. Extra right brackets, say {}]]]
        3. Some left brackets do not match with right brackets, say [{(]}]
    2. Iterate the whole string. Once we find a left bracket, we append the corresponding right bracket to the stack list. Once we find a right bracket, we compare this right bracket with the top of the stack (the last added element in the list) to see if they match (case c).
    3. If the current character is a right bracket but the stack is empty already, return False (case b)
    4. In the end, if the stack list is not empty (right brackets do not fully cancelled all left brackets), return False (case a).

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 2 |
    | Mastery | ❚❚ |
    | The Latest Attempt | January 9, 2026 |

---

[Back to Day 9 Stack & Queue I](index.md) · [All Algorithm notes](../index.md)
