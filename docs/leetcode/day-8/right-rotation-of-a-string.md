---
hide:
  - toc
---

# Right Rotation of a String

*Edited by Charles*

**Medium · Python · String**

??? question "Problem"

    [Original source](https://programmercarl.com/kamacoder/0055.%E5%8F%B3%E6%97%8B%E5%AD%97%E7%AC%A6%E4%B8%B2.html)

    ### **Problem Title: Right Rotation of a String**

    **Description:**
    The right rotation operation of a string involves moving a certain number of characters from the end of the string to the front. Given a string `s` and a positive integer `k`, please write a function to move the last `k` characters of the string to the front, achieving the right rotation of the string.

    For example, given the input string `"abcdefg"` and the integer `k = 2`, the function should convert it to `"fgabcde"`.



    **Input Format:**
    The input consists of two lines:

    1. The first line is a positive integer `k`, representing the number of positions to rotate.
    2. The second line is the string `s`, representing the string to be rotated.

    **Output Format:**
    The output consists of a single line: the string after the right rotation operation.

    **Sample Input:**

    2
    abcdefg

    **Sample Output**:

    fgabcde

    **Constraints:**

    - `1 <= k < 10000`
    - `1 <= s.length < 10000`

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        Move the last `k` characters to the front with slicing. Reduce `k` modulo the string length to support rotations larger than the string.

    Original study notes follow, including intermediate snippets and earlier attempts.

    String in Python is immutable, and this means we still need extra space.

    In other languages, if it is not allowed to apply for new spaces, we can solve the problem in this way:

    1. reverse the whole string
    2. reverse two subarrays ([0:k] and [k:])

    ```python
    s = s[-k:] + s[:-k]
    ```

    !!! note "Import correction"

        The main implementation turns the original slicing idea into a runnable function, with modulo handling for large rotations. The original sketch is retained below.

??? note "Original implementation draft"

    ```text
    s = 'abcdefg' k = 2
    def reverse():
    s.reverse() #
    'gfedcba' -> 'fg edcba' -> 'fg abcde'
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 7, 2026 |

## Python solution

```python
def rotate_right(s: str, k: int) -> str:
    if not s:
        return s
    k %= len(s)
    return s[-k:] + s[:-k]


if __name__ == "__main__":
    k = int(input())
    s = input()
    print(rotate_right(s, k))
```

---

[Back to Day 8 String II](index.md) · [All Algorithm notes](../index.md)
