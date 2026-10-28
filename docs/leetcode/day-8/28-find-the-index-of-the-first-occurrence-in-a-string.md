---
hide:
  - toc
---

# 28. Find the Index of the First Occurrence in a String

*Edited by Charles*

**Easy · Python · String**

[Original problem](https://programmercarl.com/0028.%E5%AE%9E%E7%8E%B0strStr.html)

!!! tip "Core idea"

    **Problem Title: Implement strStr()**

    **Description:**
    Implement the `strStr()` function.

## Python solution

```python
class Solution:
		def strStr(self, haystack, needle):
				n = len(needle)
				if n == 0:
						return 0
				next = [0] * n
				self.getNext(next, needle)

				j = 0
				m = len(haystack)
				for i in range(m):
						while j > 0 and haystack[i] != needle[j]:
								j = next[j - 1]
						if haystack[i] == needle[j]:
								j += 1
						if j == n:
							# In this scenario, takex
						# haystack = 'aabaab', needle = 'aab'
						# as an example
						# j == n is triggered when i = 2, j = 3
						# that is, two strings are matched from 0 to 2
						# but j always goes to next number first
						# hence, we have j = 3 in the iteration of i = 2
						# the start index should be 0, i.e. 2 - 3 + 1
								return i - n + 1

				return -1

		def getNext(self, next, s):
				j = 0
				next[0] = 0
				for i in range(1, len(s)):
						while j > 0 and s[i] != s[j]:
								j = next[j - 1]

						if s[i] == s[j]:
								j += 1

						next[i] = j
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    **Problem Title: Implement strStr()**

    **Description:**
    Implement the `strStr()` function.

    Given two strings, `haystack` and `needle`, find the first occurrence of the `needle` string in the `haystack` string (starting from index 0). If `needle` is not part of `haystack`, return `-1`.

    **Example 1:**

    - **Input**: `haystack = "hello"`, `needle = "ll"`
    - **Output**: `2`

    **Example 2:**

    - **Input**: `haystack = "aaaaa"`, `needle = "bba"`
    - **Output**: `-1`

    **Clarification:**
    What should we return when `needle` is an empty string? This is a great question to ask during an interview. For the purpose of this problem, we will return `0` when `needle` is an empty string. This is consistent with C's `strstr()` and Java's `indexOf()`.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 7, 2026 |

---

[Back to Day 8 String II](index.md) · [All Algorithm notes](../index.md)
