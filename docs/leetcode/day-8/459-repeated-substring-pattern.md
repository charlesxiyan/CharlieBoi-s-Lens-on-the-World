---
hide:
  - toc
---

# 459. Repeated Substring Pattern

*Edited by Charles*

**Easy · Python · String**

[Original problem](https://leetcode.com/problems/repeated-substring-pattern/description/)

!!! tip "Core idea"

    See the Python implementation and detailed notes below.

## Python solution

```python
class Solution:
    def getNext(self, next, s):
        j = 0
        next[0] = 0
        n = len(s)

				# ABAC ABAB
        for i in range(1, n):
            while j > 0 and s[i] != s[j]:
                j = next[j - 1]
            if s[i] == s[j]:
                j += 1
            next[i] = j

    def repeatedSubstringPattern(self, s: str) -> bool:
        m = len(s)
        next = [0] * m
        self.getNext(next, s)


				# s = 'abcabcabc'
				# longest prefix: abcabc (0->5)
				# longest suffix: abcabc (3->8)
				# this reveals that s[0:3] == s[6:9]
				# 3 is the minimum length of the repeated unit
				# i.e. the string consists of multiple these units
				# Thus, we cannot only check next[-1] != 0, since
				# 'aabaabaa' -> myNext = [0, 1, 0, 1, 2, 3, 4, 5]
				# does not have a repeated substring.

        if next[-1] != 0 and m % (m - next[-1]) == 0:
            return True

        return False
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.



??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 7, 2026 |

---

[Back to Day 8 String II](index.md) · [All Algorithm notes](../index.md)
