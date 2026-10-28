---
hide:
  - toc
---

# 17. Letter Combination of a Phone Number

*Edited by Charles*

**Medium · Python · Backtrack**

[Original problem](https://leetcode.com/problems/letter-combinations-of-a-phone-number/description/)

!!! tip "Core idea"

    Recall [77. Combinations](77-combinations.md) for the core idea. The trick part in the solution is to initiate a list of letters: parse empty strings to 0 and 1, so that the rest index correspond to letters shown on the keys.

## Python solution

```python
class Solution:
    def __init__(self):
        self.lettermap = [
            "",  # 0
            "",  # 1
            "abc",  # 2
            "def",
            "ghi",
            "jkl",
            "mno",
            "pqrs",
            "tuv",
            "wxyz"
        ]

    def letterCombinations(self, digits: str) -> List[str]:
        result = []
        if len(digits) == 0:
            return result

        self.backtrack(digits, 0, [], result)
        return result

    def backtrack(self, digits, startIndex, path, result):
        if startIndex == len(digits):
            result.append(''.join(path))
            return

        digit = int(digits[startIndex])
        letters = self.lettermap[digit]

        for letter in letters:
            path.append(letter)
            self.backtrack(digits, startIndex + 1, path, result)
            path.pop()
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. Recall [77. Combinations](77-combinations.md) for the core idea.
    2. The trick part in the solution is to initiate a list of letters: parse empty strings to 0 and 1, so that the rest index correspond to letters shown on the keys.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 21, 2026 |

---

[Back to Day 19 Backtrack I](index.md) · [All Algorithm notes](../index.md)
