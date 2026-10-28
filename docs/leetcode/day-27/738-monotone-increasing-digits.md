---
hide:
  - toc
---

# 738. Monotone Increasing Digits

*Edited by Charles*

**Medium · Python · Greedy**

[Original problem](https://leetcode.com/problems/monotone-increasing-digits/description/)

!!! tip "Core idea"

    The logic is once the right digit ‘r’ is less than the left digit ‘l’, change them locally to digits ‘(l - 1)9’. There pops up a problem. Take 332 as an example. If we iterate digits from left to right. For digits ‘33’, we do nothing; for digits ‘32’, we update it to ‘29’. Thus, iteration from left to right does not work properly since it sometimes sabotages the well-adjusted-and-iterated digit pairs.

## Python solution

```python
class Solution:
    def monotoneIncreasingDigits(self, n: int) -> int:
        s = str(n)
        l = len(s)

        for i in range(l - 1, 0, -1):
            if s[i - 1] > s[i]:
                s = s[: i - 1] + str(int(s[i - 1]) - 1) + '9' * (l - i)

        return int(s)
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. The logic is once the right digit ‘r’ is less than the left digit ‘l’, change them locally to digits ‘(l - 1)9’.
    2. There pops up a problem. Take 332 as an example. If we iterate digits from left to right. For digits ‘33’, we do nothing; for digits ‘32’, we update it to ‘29’. Thus, iteration from left to right does not work properly since it sometimes sabotages the well-adjusted-and-iterated digit pairs.
    3. Then, the iteration from right to left would solve this concern. Once the pair is done, reconstruct the whole digit string by concatenation.
    4. Attention: we cannot assign characters to a string to make an update, since it is not allowed in Python.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 30, 2026 |

---

[Back to Day 27 Greedy Algorithm V](index.md) · [All Algorithm notes](../index.md)
