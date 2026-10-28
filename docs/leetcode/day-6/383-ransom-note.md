---
hide:
  - toc
---

# 383. Ransom Note

*Edited by Charles*

**Easy · Python · HashTable**

[Original problem](https://leetcode.com/problems/ransom-note/description/)

!!! tip "Core idea"

    Count the available characters in `magazine`. Every character required by `ransomNote` must have a sufficient count.

## Python solution

```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        set_r = {char: ransomNote.count(char) for char in set(ransomNote)}
        set_m = {char: magazine.count(char) for char in set(magazine)}

        for char in set_r:
            if char in set_m and set_r[char] <= set_m[char]:
                continue
            else:
                return False

        return True
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    The main idea of the solution is

    1. count each character in ransomNote and magazine.
    2. True:
        1. characters in ransomNote are also in magazine.
        2. number of characters in ransomNote is no greater than the number in magazine.

        If either a or b fails, False.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 5, 2026 |

---

[Back to Day 6 Hash Table II](index.md) · [All Algorithm notes](../index.md)
