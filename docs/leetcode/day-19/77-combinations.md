---
hide:
  - toc
---

# 77. Combinations

*Edited by Charles*

**Medium · Python · Backtrack**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/combinations/description/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"
        Build each combination in increasing order. Before choosing the next
        number, check whether enough numbers remain to reach size `k`.
        Skip candidates that cannot possibly complete the current path.

    ## Why pruning is possible

    The base case returns as soon as `len(path) == k`. Otherwise, the path still
    needs `k - len(path)` numbers. The recursive call uses `i + 1` as its next
    starting point, so after choosing `i`, only larger numbers may be added.

    If too few numbers remain, exploring that branch cannot produce a complete
    combination. Larger candidates leave even fewer choices, so the loop can
    stop before those candidates without losing any valid result.

    ## Deriving the loop's upper bound

    **1. Count how many numbers are still needed.** Before appending the current
    candidate `i`, let

    $$\text{needed}=k-\operatorname{len}(\text{path}).$$

    This count **includes `i` itself**. We have not appended it yet.

    **2. Count how many numbers are available.** If `i` is the next chosen number,
    all remaining choices must come from

    $$i,i+1,\ldots,n.$$

    This inclusive interval contains $n-i+1$ numbers. A necessary condition for
    completing the combination is therefore

    $$n-i+1\ge k-\operatorname{len}(\text{path}).$$

    For this problem, the condition is also sufficient to leave some completion:
    any required number of larger elements may be chosen from the available range.

    **3. Solve for the largest valid candidate.** Rearranging gives

    $$
    \begin{aligned}
    n+1&\ge i+k-\operatorname{len}(\text{path}),\\
    i&\le n+1-\bigl(k-\operatorname{len}(\text{path})\bigr).
    \end{aligned}
    $$

    **4. Convert the inclusive bound to Python's exclusive endpoint.**
    `range(startIndex, stop)` includes `startIndex` but excludes `stop`.
    To include the largest valid candidate, set `stop` to that candidate plus one:

    $$
    \text{stop}
    =\underbrace{n+1-\bigl(k-\operatorname{len}(\text{path})\bigr)}_{\text{largest valid }i}
    +1
    =n+2-\bigl(k-\operatorname{len}(\text{path})\bigr).
    $$

    This produces the loop used in the solution:

    ```python
    for i in range(startIndex, n + 2 - (k - len(path))):
    ```

    ## Example: n = 5, k = 3, path = [1]

    Here `startIndex = 2` and we still need $3-1=2$ numbers, including the next
    candidate. The largest valid candidate is $5+1-2=4$, so the loop is
    `range(2, 5)`: it visits `2`, `3`, and `4`.

    | Candidate | Available numbers, including the candidate | Enough for two slots? | Result |
    | --- | --- | --- | --- |
    | `i = 4` | `[4, 5]` — two numbers | Yes | Choose `4`, then `5`, to obtain `[1, 4, 5]`. |
    | `i = 5` | `[5]` — one number | No | The branch cannot reach three elements and is pruned. |

    !!! warning "The two +1 terms have different jobs"
        The `+1` in `n - i + 1` counts **both endpoints** of the available interval.
        The additional `+1` when forming `range()` includes the largest valid
        candidate despite Python's **exclusive** stop value. Using
        `range(startIndex, n + 1 - (k - len(path)))` would omit `i = 4` in this
        example and incorrectly lose `[1, 4, 5]`.

    ## Original study notes

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. Recall [113. Path Sum II](../day-14/113-path-sum-ii.md). The reason why we have result.append(path[:]) is that path[:] works the same as list() in [113. Path Sum II](../day-14/113-path-sum-ii.md) so that path.pop() later would not influence the lists that have already been appended in result.
    2. Backtrack solution: 3 Steps
        1. determine input arguments and the return value of the recursive function.
        2. determine the terminal condition
        3. determine the logic of the recurrence
    3. Pruning:
        1. consider n = 4, k = 4.
        2. The numbers to be chosen are 1, 2, 3, 4. As the startIndex starts from 1, the recurrence within this iteration (startIndex = 1) will return one valid path.
        3. When startIndex iterates to 2 or greater values, they won’t return valid paths, since they even cannot make the len(path) = 4, e.g. [2, 3, 4].
        4. Thus, the recurrence within the iteration should determine the upper bound of the iteration, n + 1 - (k - len(path)) + 1 = n + 2 - (k - len(path))
        5. The upper boundary of i: i <= n + 1 - (k - len(path)), i.e. in n + 2 - (k - len(path)) range.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 21, 2026 |

## Python solution

```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        result = []
        self.backtrack(n, k, 1, [], result)
        return result

    def backtrack(self, n, k, startIndex, path, result):
        if len(path) == k:
            result.append(path[:])
            return
				# Pruning: n - i + 1 >= k - len(path)
				# there are n - (i - 1) numbers as options to choose; now we have k - len(path) numbers left to be filled.
				# We need to make sure n - (i - 1) >= k - len(path)
        for i in range(startIndex, n + 2 - (k - len(path))):
            path.append(i)
            self.backtrack(n, k, i + 1, path, result)
            path.pop()
```

---

[Back to Day 19 Backtrack I](index.md) · [All Algorithm notes](../index.md)
