---
hide:
  - toc
---

# 860. Lemonade Change

*Edited by Charles*

**Easy · Python · Greedy**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/lemonade-change/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Track the counts of $5 and $10 bills. Give change as each customer arrives, using a $10 bill before spending three $5 bills when possible.

## Python solution

```python
class Solution:
    def lemonadeChange(self, bills: List[int]) -> bool:
        five = ten = 0
        for b in bills:
            if b == 5:
                five += 1
            elif b == 10:
                five -= 1
                ten += 1

            # b = 20 case
            elif ten > 0:
                five -= 1
                ten -= 1
            else:
                five -= 3

            if five < 0:
                return False
        return True
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. Iterate the list bills.
        1. If the current note is $5, collect it.
        2. If the current note is $10, check if we have at least one $5 note.
            1. If yes, collect it and dock one $5 note.
            2. If no, return False.
        3. If the current note is $20, check if we have at least one $5 note and one $10 note first.
            1. If yes, collect it and dock one $5 note and one $10 note.
            2. If no, check if we have at least 3 $5 notes.
                1. If yes, collect it and dock three $5 notes.
                2. If no, return False.
        4. The cases above include all scenarios for the first note.

    ```python
    class Solution:
        def lemonadeChange(self, bills: List[int]) -> bool:
            cash = {5: 0, 10: 0, 20:0}
            n = len(bills)

            for i in range(n):
                if bills[i] == 5:
                    cash[5] += 1

                if bills[i] == 10:
                    if cash[5] >= 1:
                        cash[5] -= 1
                        cash[10] += 1
                    else:
                        return False

                if bills[i] == 20:
                    if cash[10] >= 1 and cash[5] >= 1:
                        cash[20] += 1
                        cash[10] -= 1
                        cash[5] -= 1
                    elif cash[10] == 0 and cash[5] >= 3:
                        cash[20] += 1
                        cash[5] -= 3
                    else:
                        return False

            return True
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 28, 2026 |

---

[Back to Day 25 Greedy Algorithm III](index.md) · [All Algorithm notes](../index.md)
