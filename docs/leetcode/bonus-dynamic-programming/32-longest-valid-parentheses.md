---
hide:
  - toc
---

# 32. Longest Valid Parentheses

*Edited by Charles*

**Hard · Python · Dynamic Programming**

[Original problem](https://leetcode.com/problems/longest-valid-parentheses/)

!!! tip "Core idea"

    Given a string `s` containing only:

    return the length of the **longest valid parentheses substring**.

## Python solution

```python
class Solution:
    def longestValidParentheses(self, s: str) -> int:
        n = len(s)

        if n == 0:
            return 0

        dp = [0] * n
        res = 0

        for i in range(1, n):
            if s[i] == ')':

                # Case 1: ...()
                if s[i - 1] == '(':
                    dp[i] = (dp[i - 2] if i >= 2 else 0) + 2

                # Case 2: ...))
                else:
                    j = i - dp[i - 1] - 1

                    if j >= 0 and s[j] == '(':
		                    # Case (())
                        dp[i] = dp[i - 1] + 2

                        if j >= 1:
                        # Case ()(())
                            dp[i] += dp[j - 1]

                res = max(res, dp[i])

        return res
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Problem

    Given a string `s` containing only:

    ```text
    '(' and ')'
    ```

    return the length of the **longest valid parentheses substring**.

    ## Example

    ```text
    Input:
    s = ")()())"

    Output:
    4
    ```

    Because the longest valid parentheses substring is:

    ```text
    "()()"
    ```

    with length:

    ```text
    4
    ```

    The most important part of this problem is the definition of `dp[i]`.

    Define:

    ```text
    dp[i] = the length of the longest valid parentheses substring
            that MUST end at index i
    ```

    The key phrase is:

    ```text
    MUST end at i
    ```

    It does **not** mean:

    ```text
    the longest valid parentheses substring in s[0:i+1]
    ```

    Consider:

    ```text
    s = ")()())"

    index:  0  1  2  3  4  5
    char:   )  (  )  (  )  )
    dp:     0  0  2  0  4  0
    ```

    At index `4`:

    ```text
    ) ( ) ( ) )
      └─────┘
        ()()
    ```

    There is a valid substring of length `4` ending at index `4`.

    Therefore:

    ```text
    dp[4] = 4
    ```

    But at index `5`:

    ```text
    )()())
         ↑
         i
    ```

    there is no valid parentheses substring ending at index `5`.

    Therefore:

    ```text
    dp[5] = 0
    ```

    So:

    ```python
    dp[-1] == 0
    ```

    even though the answer is `4`.

    Therefore, the final answer is:

    ```python
    max(dp)
    ```

    rather than:

    ```python
    dp[-1]
    ```

    A valid parentheses substring must end with:

    ```text
    )
    ```

    It cannot end with:

    ```text
    (
    ```

    Therefore, if:

    ```python
    s[i] == '('
    ```

    then:

    ```python
    dp[i] = 0
    ```

    We only need to process positions where:

    ```python
    s[i] == ')'
    ```

    Suppose:

    ```text
    ...()
       ↑↑
     i-1 i
    ```

    That is:

    ```python
    s[i - 1] == '('
    s[i] == ')'
    ```

    Then:

    ```text
    s[i-1:i+1] = "()"
    ```

    which gives us a new valid pair of length `2`.

    But there may already be a valid parentheses substring immediately before this pair.

    For example:

    ```text
    ()()
    ```

    At the last `)`:

    ```text
    index:  0 1 2 3
    char:   ( ) ( )
                  ↑
                  i
    ```

    The current:

    ```text
    ()
    ```

    contributes:

    ```text
    2
    ```

    and the previous valid substring contributes:

    ```text
    dp[i - 2]
    ```

    Therefore:

    ```python
    dp[i] = dp[i - 2] + 2
    ```

    If `i < 2`, there is nothing before the current `()`.

    So we write:

    ```python
    dp[i] = (dp[i - 2] if i >= 2 else 0) + 2
    ```

    Now consider:

    ```text
    ...))
       ↑↑
     i-1 i
    ```

    We cannot directly match:

    ```text
    ))
    ```

    However, `dp[i-1]` may tell us that there is already a valid parentheses substring ending at `i-1`.

    For example:

    ```text
    (())
    ```

    At the final `)`:

    ```text
    index:  0 1 2 3
    char:   ( ( ) )
                  ↑
                  i
    ```

    We already know:

    ```text
    dp[i - 1] = dp[2] = 2
    ```

    because:

    ```text
    ()
    ```

    ends at index `2`.

    Graphically:

    ```text
    ( ( ) )
    ↑ └─┘ ↑
    j     i
    ```

    We want to find the character immediately before the valid substring ending at `i-1`.

    The valid substring ending at `i-1` has length:

    ```text
    dp[i - 1]
    ```

    So we jump backward by that many characters and then one additional position:

    ```python
    j = i - dp[i - 1] - 1
    ```

    This `j` represents:

    > The position that might contain the `(` matching the current `s[i] == ')'`.
    >

    For:

    ```text
    (())
    ```

    we have:

    ```text
    i = 3
    dp[i - 1] = 2

    j = 3 - 2 - 1
      = 0
    ```

    So:

    ```text
    index:  0 1 2 3
    char:   ( ( ) )
            ↑ └─┘ ↑
            j     i
    ```

    Now:

    ```python
    s[j] == '('
    ```

    so `s[j]` can match the current `s[i]`.

    If:

    ```python
    j >= 0 and s[j] == '('
    ```

    then we can combine:

    ```text
    ( + valid substring + )
    ```

    The existing valid substring has length:

    ```python
    dp[i - 1]
    ```

    The new outer pair contributes:

    ```text
    2
    ```

    Therefore:

    ```python
    dp[i] = dp[i - 1] + 2
    ```

    But there is one more important case.

    Consider:

    ```text
    ()(())
    ```

    The final `(())` is valid, but there is also another valid substring before it:

    ```text
    () + (())
    ```

    We want to combine them into:

    ```text
    ()(())
    ```

    So after finding `j`, we also need to check whether there is another valid substring ending at:

    ```text
    j - 1
    ```

    Therefore:

    ```python
    dp[i] = dp[i - 1] + 2 + dp[j - 1]
    ```

    If `j == 0`, then there is nothing before `j`.

    So:

    ```python
    dp[i] = dp[i - 1] + 2

    if j >= 1:
        dp[i] += dp[j - 1]
    ```

    Consider:

    ```text
    () (())
    ^^ ^^^^
    ```

    The current structure is:

    ```text
    [previous valid] ( [current valid] )
    ```

    The part:

    ```text
    ( [current valid] )
    ```

    gives:

    ```python
    dp[i - 1] + 2
    ```

    But there may also be another valid substring immediately before the matching `(`.

    That substring ends at:

    ```text
    j - 1
    ```

    Its length is:

    ```python
    dp[j - 1]
    ```

    Therefore:

    ```text
    dp[i]
    =
    previous valid substring
    +
    current inner valid substring
    +
    new pair

    =
    dp[j - 1]
    +
    dp[i - 1]
    +
    2
    ```

    The entire problem can be summarized into two cases.

    ## Case 1

    ```text
    ...()
       ↑↑
     i-1 i
    ```

    If:

    ```python
    s[i - 1] == '('
    ```

    then:

    ```python
    dp[i] = dp[i - 2] + 2
    ```

    ## Case 2

    ```text
    ...(valid))
               ↑
               i
    ```

    If:

    ```python
    s[i - 1] == ')'
    ```

    first find:

    ```python
    j = i - dp[i - 1] - 1
    ```

    If:

    ```python
    j >= 0 and s[j] == '('
    ```

    then:

    ```python
    dp[i] = dp[i - 1] + 2 + dp[j - 1]
    ```

    where `dp[j - 1]` is included only when:

    ```text
    j >= 1
    ```

    ```text
    s[i] == ')'
    │
    ├── Case 1: s[i-1] == '('
    │
    │       ... ( )
    │           ↑ ↑
    │          i-1 i
    │
    │       dp[i]
    │       = dp[i-2] + 2
    │
    │
    └── Case 2: s[i-1] == ')'

            ... ( [valid] )
                ↑         ↑
                j         i

            j = i - dp[i-1] - 1

            if s[j] == '(':

            dp[i]
            = dp[i-1] + 2 + dp[j-1]
    ```

    Consider:

    ```text
    s = ")()())"

    index:  0  1  2  3  4  5
    char:   )  (  )  (  )  )
    ```

    Initially:

    ```text
    dp = [0, 0, 0, 0, 0, 0]
    ```

    ## i = 1

    ```text
    s[1] = '('
    ```

    A valid parentheses substring cannot end with `(`.

    Therefore:

    ```text
    dp[1] = 0
    ```

    ## i = 2

    ```text
    s[2] = ')'
    s[1] = '('
    ```

    This is Case 1:

    ```text
    ()
    ```

    Therefore:

    ```python
    dp[2] = dp[0] + 2
          = 2
    ```

    Now:

    ```text
    dp = [0, 0, 2, 0, 0, 0]
    ```

    ## i = 3

    ```text
    s[3] = '('
    ```

    Therefore:

    ```text
    dp[3] = 0
    ```

    ## i = 4

    ```text
    s[4] = ')'
    s[3] = '('
    ```

    Again, Case 1:

    ```python
    dp[4] = dp[2] + 2
          = 2 + 2
          = 4
    ```

    Now:

    ```text
    dp = [0, 0, 2, 0, 4, 0]
    ```

    The valid substring ending at index `4` is:

    ```text
    ) ( ) ( ) )
      └─────┘
        ()()
    ```

    ## i = 5

    ```text
    s[5] = ')'
    s[4] = ')'
    ```

    This is Case 2.

    Calculate:

    ```python
    j = i - dp[i - 1] - 1
    ```

    So:

    ```text
    j = 5 - dp[4] - 1
      = 5 - 4 - 1
      = 0
    ```

    But:

    ```text
    s[0] = ')'
    ```

    not:

    ```text
    '('
    ```

    Therefore, the current `)` cannot form a valid substring ending at index `5`.

    So:

    ```text
    dp[5] = 0
    ```

    Final DP array:

    ```text
    index:  0  1  2  3  4  5
    char:   )  (  )  (  )  )
    dp:     0  0  2  0  4  0
    ```

    Therefore:

    ```python
    max(dp) = 4
    ```

    This is an important detail.

    Our definition is:

    ```text
    dp[i]
    =
    longest valid parentheses substring
    that MUST end at index i
    ```

    Therefore:

    ```text
    dp[-1]
    ```

    only tells us the longest valid substring that ends at the **last character**.

    It does not represent the maximum over the entire string.

    For:

    ```text
    ")()())"
    ```

    we have:

    ```text
    dp = [0, 0, 2, 0, 4, 0]
    ```

    So:

    ```python
    dp[-1] == 0
    ```

    but:

    ```python
    max(dp) == 4
    ```

    Therefore, the answer is:

    ```python
    max(dp)
    ```

    or we can maintain a global maximum:

    ```python
    res = max(res, dp[i])
    ```

    ## Time Complexity

    We traverse the string once:

    ```text
    O(n)
    ```

    ## Space Complexity

    We use a DP array of length `n`:

    ```text
    O(n)
    ```



    The most important definition is:

    ```text
    dp[i]
    =
    the length of the longest valid parentheses substring
    ending exactly at index i
    ```

    Therefore:

    ```text
    s[i] == '('
    → dp[i] = 0
    ```

    When:

    ```text
    s[i] == ')'
    ```

    there are two cases:

    ```text
    Case 1:

    ...()

    dp[i] = dp[i-2] + 2
    ```

    and:

    ```text
    Case 2:

    ... ( [valid] )
        ↑         ↑
        j         i

    j = i - dp[i-1] - 1
    ```

    If:

    ```text
    s[j] == '('
    ```

    then:

    ```text
    dp[i]
    =
    dp[i-1] + 2 + dp[j-1]
    ```

    Finally, remember:

    ```text
    dp[i] = best valid substring ENDING at i
    ```

    so the final answer is:

    ```python
    max(dp)
    ```

    not:

    ```python
    dp[-1]
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | September 17, 2026 |

---

[Back to Bonus Dynamic Programming](index.md) · [All Algorithm notes](../index.md)
