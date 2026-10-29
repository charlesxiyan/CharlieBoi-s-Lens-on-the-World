---
hide:
  - toc
---

# 151. Reverse Words in a String

*Edited by Charles*

**Medium · Python · String**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/reverse-words-in-a-string/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        Reference to [344. Reverse String](../day-7/344-reverse-string.md)

    Original study notes follow, including intermediate snippets and earlier attempts.

    Reference to [344. Reverse String](../day-7/344-reverse-string.md)

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 7, 2026 |

## Python solution

```python
class Solution:
    def reverseWords(self, s: str) -> str:
        words = s.split()
        words = words[::-1]
        return ' '.join(words)

        # Alternatively, double pointers
        # n = len(words)
        # left, right = 0, n - 1
        # while left < right:
        #     words[left], words[right] = words[right], words[left]
        #     left += 1
        #     right -= 1

        # return ' '.join(words)
```

---

[Back to Day 8 String II](index.md) · [All Algorithm notes](../index.md)
