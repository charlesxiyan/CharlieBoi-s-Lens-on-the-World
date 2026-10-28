---
hide:
  - toc
---

# 150. Evaluate Reverse Polish Notation

*Edited by Charles*

**Medium · Python · Stack**

[Original problem](https://leetcode.com/problems/evaluate-reverse-polish-notation/)

!!! tip "Core idea"

    The operator always comes when the stack has enough numbers to calculate. Iterate the whole list, append the element to the stack if it is a digit character; else, pop two elements out and complete the calculation.

## Python solution

```python
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        n = len(tokens)
        stack = []
        operators = ['+', '-', '*', '/']

        for i in range(n):

            if tokens[i] not in operators:
                stack.append(int(tokens[i]))

            else:
                b = stack.pop()
                a = stack.pop()

                if tokens[i] == operators[0]:
                    stack.append(a + b)
                if tokens[i] == operators[1]:
                    stack.append(a - b)
                if tokens[i] == operators[2]:
                    stack.append(a * b)
                if tokens[i] == operators[3]:
                    stack.append(int(a / b))

        return stack.pop()
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. The operator always comes when the stack has enough numbers to calculate.
    2. Iterate the whole list, append the element to the stack if it is a digit character; else, pop two elements out and complete the calculation.
    3. For the division, use int(a / b) to make sure the result is the integer part.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 9, 2026 |

---

[Back to Day 10 Stack & Queue II](index.md) · [All LeetCode notes](../index.md)
