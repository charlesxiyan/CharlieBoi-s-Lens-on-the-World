---
hide:
  - toc
---

# 202. Happy Number

*Edited by Charles*

**Easy · Python · HashTable**

[Original problem](https://leetcode.com/problems/happy-number/description/)

!!! tip "Core idea"

    Keep in mind that the question requires to return True if the summation of all digits is 1; False if the summation is one of the past summations, i.e. endless cycle. Thus, we need to set the condition of the loop as a check on the new number if it pops up before.

## Python solution

```python
class Solution:
    def isHappy(self, n: int) -> bool:
        record = []

        while n not in record:
            record.append(n)
            sum_digits = 0
            n_str = str(n)

            for digit in n_str:
                sum_digits += int(digit) ** 2

            if sum_digits == 1:
                return True
            else:
                n = sum_digits

        return False
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    1. Keep in mind that the question requires to return True if the summation of all digits is 1; False if the summation is one of the past summations, i.e. endless cycle.
    2. Thus, we need to set the condition of the loop as a check on the new number if it pops up before.
    3. Initialize a list to record all numbers.
    4. Within the while loop, string the number and then sum each digit (convert it to integer again) up to generate a new number. If it is 1, return False; if not, pass the number to the variable in the while loop condition.
    5. Once the while loop is broken, that means an endless cycle has been found it is impossible for the initial number to be a happy number.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 5, 2026 |

---

[Back to Day 5 Hash Table I](index.md) · [All LeetCode notes](../index.md)
