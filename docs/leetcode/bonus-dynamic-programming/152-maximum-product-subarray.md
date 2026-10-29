---
hide:
  - toc
---

# 152. Maximum Product Subarray

*Edited by Charles*

**Medium · Python · Dynamic Programming**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/maximum-product-subarray/description/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        Given an integer array `nums`, find a contiguous non-empty subarray that has the largest product, and return the product.

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Problem

    Given an integer array `nums`, find a contiguous non-empty subarray that has the largest product, and return the product.

    ## Example 1

    ```text
    Input:
    nums = [2, 3, -2, 4]

    Output:
    6
    ```

    Because:

    ```text
    [2, 3]
    ```

    has the maximum product:

    ```text
    2 × 3 = 6
    ```

    ## Example 2

    ```text
    Input:
    nums = [-2, 0, -1]

    Output:
    0
    ```

    The result cannot be:

    ```text
    2
    ```

    because:

    ```text
    [-2, -1]
    ```

    is not a contiguous subarray.

    This problem is similar to the Maximum Subarray problem, but multiplication introduces one important complication:

    > A negative number can turn the smallest negative product into the largest positive product.
    >

    Therefore, we cannot only keep track of the maximum product.

    We need to maintain **both the maximum and minimum product ending at each position**.

    Define:

    ```text
    maxDp[i]
    =
    maximum product of a subarray
    that MUST end at nums[i]
    ```

    and:

    ```text
    minDp[i]
    =
    minimum product of a subarray
    that MUST end at nums[i]
    ```

    The key phrase is:

    ```text
    MUST end at i
    ```

    For example:

    ```text
    nums = [2, 3, -2, 4]
    ```

    `maxDp[1]` represents the maximum product among all subarrays ending at `nums[1] = 3`:

    ```text
    [3]      → 3
    [2, 3]   → 6
    ```

    Therefore:

    ```text
    maxDp[1] = 6
    ```

    This is the key idea of LeetCode 152.

    Consider:

    ```text
    nums = [-2, 3, -4]
    ```

    At `3`, the minimum product ending there is:

    ```text
    [-2, 3]

    -2 × 3 = -6
    ```

    So:

    ```text
    minDp[1] = -6
    ```

    Now we encounter:

    ```text
    -4
    ```

    The previous minimum product:

    ```text
    -6
    ```

    multiplied by:

    ```text
    -4
    ```

    becomes:

    ```text
    (-6) × (-4) = 24
    ```

    which becomes the new **maximum**.

    Therefore:

    ```text
    negative × negative = positive
    ```

    means that the previous minimum value may become the current maximum value.

    Similarly, the previous maximum value may become the current minimum value.

    So we must track:

    ```text
    maxDp
    +
    minDp
    ```

    at the same time.

    At position `i`, any subarray ending at `nums[i]` has three possibilities.

    ## Case 1 — Start a New Subarray

    Ignore everything before `i` and start directly from:

    ```text
    [nums[i]]
    ```

    The product is:

    ```python
    nums[i]
    ```

    ## Case 2 — Extend the Previous Maximum Product

    Take the previous maximum product:

    ```python
    maxDp[i - 1]
    ```

    and multiply by the current number:

    ```python
    maxDp[i - 1] * nums[i]
    ```

    ## Case 3 — Extend the Previous Minimum Product

    Take the previous minimum product:

    ```python
    minDp[i - 1]
    ```

    and multiply by:

    ```python
    nums[i]
    ```

    So:

    ```python
    minDp[i - 1] * nums[i]
    ```

    This case is especially important when `nums[i]` is negative.

    Among the three possibilities:

    ```text
    nums[i]

    maxDp[i - 1] × nums[i]

    minDp[i - 1] × nums[i]
    ```

    take the maximum:

    ```python
    maxDp[i] = max(
        maxDp[i - 1] * nums[i],
        minDp[i - 1] * nums[i],
        nums[i]
    )
    ```

    Similarly, take the minimum of the same three possibilities:

    ```python
    minDp[i] = min(
        maxDp[i - 1] * nums[i],
        minDp[i - 1] * nums[i],
        nums[i]
    )
    ```

    Therefore, the two transitions are:

    ```python
    minDp[i] = min(
        maxDp[i - 1] * nums[i],
        minDp[i - 1] * nums[i],
        nums[i]
    )

    maxDp[i] = max(
        maxDp[i - 1] * nums[i],
        minDp[i - 1] * nums[i],
        nums[i]
    )
    ```

    This is another important detail.

    Why don't we only compare:

    ```python
    maxDp[i - 1] * nums[i]
    minDp[i - 1] * nums[i]
    ```

    ?

    Because sometimes the best choice is to **discard the previous subarray and start a new subarray from the current element**.

    For example:

    ```text
    nums = [-2, 3]
    ```

    At:

    ```text
    nums[1] = 3
    ```

    we have:

    ```text
    maxDp[0] = -2
    minDp[0] = -2
    ```

    If we only extend the previous subarray:

    ```text
    -2 × 3 = -6
    ```

    But starting a new subarray gives:

    ```text
    [3]

    product = 3
    ```

    Clearly:

    ```text
    3 > -6
    ```

    Therefore:

    ```python
    nums[i]
    ```

    must be one of the candidates.

    Consider:

    ```text
    nums = [-2, 0, -1]
    ```

    At:

    ```text
    nums[i] = 0
    ```

    all extended products become:

    ```text
    something × 0 = 0
    ```

    and starting from the current number also gives:

    ```text
    0
    ```

    Therefore:

    ```text
    maxDp[i] = 0
    minDp[i] = 0
    ```

    In this way, `0` naturally breaks the previous product chain.

    We do not need any special handling for zero.

    For the first element:

    ```python
    minDp[0] = nums[0]
    maxDp[0] = nums[0]
    ```

    because there is only one non-empty subarray ending at index `0`:

    ```text
    [nums[0]]
    ```

    So both the maximum and minimum products are:

    ```text
    nums[0]
    ```

    In the code:

    ```python
    minDp = [-float('inf')] * l
    maxDp = [-float('inf')] * l

    minDp[0], maxDp[0] = nums[0], nums[0]
    ```

    The initial `-inf` values are overwritten as we process the array; the important initialization is:

    ```python
    minDp[0] = maxDp[0] = nums[0]
    ```

    Consider:

    ```text
    nums = [2, 3, -2, 4]
    ```

    Initially:

    ```text
    minDp[0] = 2
    maxDp[0] = 2
    ```

    So:

    ```text
    index:    0
    nums:     2
    minDp:    2
    maxDp:    2
    ```

    ## i = 1, `nums[i] = 3`

    Three candidates:

    ```text
    maxDp[0] × 3 = 2 × 3 = 6

    minDp[0] × 3 = 2 × 3 = 6

    3
    ```

    Therefore:

    ```text
    minDp[1] = min(6, 6, 3)
             = 3

    maxDp[1] = max(6, 6, 3)
             = 6
    ```

    Now:

    ```text
    index:    0   1
    nums:     2   3
    minDp:    2   3
    maxDp:    2   6
    ```

    ## i = 2, `nums[i] = -2`

    Three candidates:

    ```text
    maxDp[1] × (-2)
    = 6 × (-2)
    = -12

    minDp[1] × (-2)
    = 3 × (-2)
    = -6

    nums[i]
    = -2
    ```

    Therefore:

    ```text
    minDp[2] = min(-12, -6, -2)
             = -12

    maxDp[2] = max(-12, -6, -2)
             = -2
    ```

    Now:

    ```text
    index:    0    1     2
    nums:     2    3    -2
    minDp:    2    3   -12
    maxDp:    2    6    -2
    ```

    Notice how:

    ```text
    maxDp[2] = -2
    ```

    because the best subarray **ending at index 2** is simply:

    ```text
    [-2]
    ```

    This does not mean the global answer has become `-2`.

    The previous:

    ```text
    [2, 3]
    ```

    still has product:

    ```text
    6
    ```

    ## i = 3, `nums[i] = 4`

    Three candidates:

    ```text
    maxDp[2] × 4
    = -2 × 4
    = -8

    minDp[2] × 4
    = -12 × 4
    = -48

    nums[i]
    = 4
    ```

    Therefore:

    ```text
    minDp[3] = -48
    maxDp[3] = 4
    ```

    Final arrays:

    ```text
    index:    0    1     2     3
    nums:     2    3    -2     4

    minDp:    2    3   -12   -48
    maxDp:    2    6    -2     4
    ```

    Remember the definition:

    ```text
    maxDp[i]
    =
    maximum product of a subarray
    that MUST end at index i
    ```

    The global maximum subarray does not necessarily end at the final position.

    For:

    ```text
    nums = [2, 3, -2, 4]
    ```

    we have:

    ```text
    maxDp = [2, 6, -2, 4]
    ```

    The maximum product is:

    ```text
    6
    ```

    which occurs at:

    ```text
    maxDp[1]
    ```

    not:

    ```text
    maxDp[-1]
    ```

    Therefore, we return:

    ```python
    return max(maxDp)
    ```

    instead of:

    ```python
    return maxDp[-1]
    ```

    For every `nums[i]`:

    ```text
                         nums[i]
                            │
               ┌────────────┼────────────┐
               ↓            ↓            ↓
          Start new     Extend max    Extend min

           nums[i]      maxDp[i-1]    minDp[i-1]
                           ×              ×
                        nums[i]        nums[i]

               \            |            /
                \           |           /
                 ───────────────────────
                            │
                   three candidates
                            │
                  ┌─────────┴─────────┐
                  ↓                   ↓
                min()               max()
                  ↓                   ↓
              minDp[i]            maxDp[i]
    ```

    When:

    ```text
    nums[i] > 0
    ```

    we usually have:

    ```text
    previous max → current max
    previous min → current min
    ```

    But when:

    ```text
    nums[i] < 0
    ```

    multiplication reverses the order:

    ```text
    previous max × negative
            ↓
    may become current minimum

    previous min × negative
            ↓
    may become current maximum
    ```

    This is why LeetCode 152 requires two DP states.

    A useful way to remember it is:

    ```text
    Addition:
    large + something usually stays related to large

    Multiplication:
    negative numbers can flip max ↔ min
    ```

    ## Time Complexity

    We traverse the array once:

    ```text
    O(n)
    ```

    The final:

    ```python
    max(maxDp)
    ```

    also takes `O(n)`, so the total remains:

    ```text
    O(n)
    ```

    ## Space Complexity

    We use two arrays of length `n`:

    ```text
    minDp
    maxDp
    ```

    Therefore:

    ```text
    O(n)
    ```

    The space complexity can be optimized to `O(1)` because each state only depends on the previous position.



    The most important state definitions are:

    ```text
    maxDp[i]
    =
    maximum product of a subarray ending exactly at i

    minDp[i]
    =
    minimum product of a subarray ending exactly at i
    ```

    At every position, there are three choices:

    ```text
    1. Start from nums[i]

    2. Extend the previous maximum product

    3. Extend the previous minimum product
    ```

    Therefore:

    ```python
    maxDp[i] = max(
        nums[i],
        maxDp[i - 1] * nums[i],
        minDp[i - 1] * nums[i]
    )
    ```

    and:

    ```python
    minDp[i] = min(
        nums[i],
        maxDp[i - 1] * nums[i],
        minDp[i - 1] * nums[i]
    )
    ```

    The reason we need both `maxDp` and `minDp` is:

    ```text
    negative × negative = positive
    ```

    so:

    ```text
    previous minimum
          ↓
    × negative number
          ↓
    may become current maximum
    ```

    Finally:

    ```python
    return max(maxDp)
    ```

    because:

    ```text
    maxDp[i] = best product ENDING at i
    ```

    and the global maximum does not necessarily end at the last position.

    The core idea to remember:

    ```text
    LeetCode 152
    =
    DP
    +
    track both maximum and minimum

    because negative numbers can flip:

    max ↔ min
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | September 18, 2026 |

## Python solution

```python
class Solution:
    def maxProduct(self, nums: list[int]) -> int:
        l = len(nums)

        minDp = [-float('inf')] * l
        maxDp = [-float('inf')] * l

        minDp[0], maxDp[0] = nums[0], nums[0]

        for i in range(1, l):
            minDp[i] = min(
                maxDp[i - 1] * nums[i],
                minDp[i - 1] * nums[i],
                nums[i]
            )

            maxDp[i] = max(
                maxDp[i - 1] * nums[i],
                minDp[i - 1] * nums[i],
                nums[i]
            )

        return max(maxDp)
```

---

[Back to Bonus Dynamic Programming](index.md) · [All Algorithm notes](../index.md)
