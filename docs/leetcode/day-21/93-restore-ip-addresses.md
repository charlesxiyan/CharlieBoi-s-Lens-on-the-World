---
hide:
  - toc
---

# 93. Restore IP Addresses

*Edited by Charles*

**Medium · Python · Backtrack**

[Original problem](https://leetcode.com/problems/restore-ip-addresses/description/)

!!! tip "Core idea"

    Pay attention to the terminal condition in the backtracking function: How to verify the validity of the IP address?

## Python solution

```python
class Solution:
    def restoreIpAddresses(self, s: str) -> List[str]:
        result = []
        self.backtrack(s, 0, [], result)
        return result

    def backtrack(self, s, startIndex, path, result):
        if startIndex == len(s) and len(path) == 4:
            result.append('.'.join(path))
            return

        if len(path) > 4:
            return

        for i in range(startIndex, len(s)):
            if self.isValid(s, startIndex, i):
                path.append(s[startIndex: i + 1])
                self.backtrack(s, i + 1, path, result)
                path.pop()

    def isValid(self, s, start, end):
        if s[start] == '0' and start != end:
            return False
        num = int(s[start: end + 1])
        return num <= 255
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. Pay attention to the terminal condition in the backtracking function:
        1. startIndex has passed the last index of the string and the length of path is only 4.
        2. if the length of the path is greater than 4, no need to go further.
        3. similar to the verification in [131. Palindrome Partitioning](../day-20/131-palindrome-partitioning.md), in the for loop, the first step is to verify if the selected substring is a valid component of the IP address.
    2. How to verify the validity of the IP address?
        1. if the start of the substring is 0, then the length of the string has to be 1, i.e. start = end; otherwise, invalid.
        2. the integer value of the substring cannot be greater than 255.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 24, 2026 |

---

[Back to Day 21 Backtrack III](index.md) · [All LeetCode notes](../index.md)
