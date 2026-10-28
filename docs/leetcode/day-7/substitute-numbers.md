---
hide:
  - toc
---

# Substitute Numbers

*Edited by Charles*

**Easy · Python · String**

[Original problem](https://kamacoder.com/problempage.php)

!!! tip "Core idea"

    **Goal:**
    Given a string `s` containing lowercase letters and digits, write a function that keeps the alphabetical characters unchanged but replaces every numeric character with the word **"number"**.

## Python solution

```python
class Solution:
		def substitute_numbers(self, s):
				n = len(s)
				count = sum(1 for char in s if char.isdigit())
				new_len = n + count * 5
				result = [''] * new_len

				new_index = new_len - 1
				old_index = n - 1

				while old_index >= 0:
						if s[old_index].isdigit():
								# fill 6 characters: number
								result[new_index - 5: new_index + 1] = 'number'
								new_index -= 6
						else:
								result[new_index] = s[old_index]
								new_index -= 1

						old_index -= 1

				return ''.join(result)
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ### **Problem Description: Replace Numbers into a String**

    **Goal:**
    Given a string `s` containing lowercase letters and digits, write a function that keeps the alphabetical characters unchanged but replaces every numeric character with the word **"number"**.

    **Example 1:**

    - **Input:** `"a1b2c3"`
    - **Output:** `"anumberbnumbercnumber"`

    **Example 2:**

    - **Input:** `"a5b"`
    - **Output:** `"anumberb"`

    **Input Format:**
    A string `s` consisting only of lowercase letters and digits.

    **Output Format:**
    Print a new string where each digit is replaced by the string `"number"`.

    **Sample Input:**`a1b2c3`

    **Sample Output:**`anumberbnumbercnumber`

    **Constraints:**`1 <= s.length < 10000`

    Note:

    1. The reason why we substitute digits into “number” from the end of the string to the start is that if we do it from start, the time complexity is $O(n^2)$.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 7, 2026 |

---

[Back to Day 7 String I](index.md) · [All LeetCode notes](../index.md)
