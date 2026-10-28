---
hide:
  - toc
---

# 746. Min Cost Climbing Stairs

*Edited by Charles*

**Easy · Python · Dynamic Programming**

[Original problem](https://leetcode.com/problems/min-cost-climbing-stairs/)

!!! tip "Core idea"

    DP Steps:

    Attention!!! The target of the question is to iterate all the elements and pass them. For example, the length of the list is n, and thus the target is to find the minimum cost to reach index n, although there is no element in index n.

## Python solution

```python
class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        n = len(cost)

        if n == 1:
            return cost[0]

        dp = [[float('inf')] * 2 for _ in range(n)]
        dp[0] = [cost[0], 0]

        for i in range(1, n):
            dp[i][0] = min(dp[i - 1][0] + cost[i], dp[i - 1][1] + cost[i])
            dp[i][1] = dp[i - 1][0]

        return min(dp[-1])
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    DP Steps:

    Attention!!! The target of the question is to iterate all the elements and pass them. For example, the length of the list is n, and thus the target is to find the minimum cost to reach index n, although there is no element in index n.

    **Step 1**: Define DP array states

    dp[i][0]: the minimum cost to reach i + 1 if the current step i is included;

    dp[i][1]: the minimum cost to reach i + 1 if the current step i is not included;

    Note that the length of dp should be n.

    **Step 2**: Recursive formula (n ≥ 1)

    dp[i][0] only comes from the minimum of two cases:

    1. dp[i - 1][0] + cost[i]: one step from last step i - 1 with the last step included
    2. dp[i - 1][1] + cost[i]: two steps from step i - 2. In breakdown, one step from step i - 2 with this step included, and one more step from step i - 1 without this step included.

    dp[i][1] only comes from one case:

    dp[i - 1][0]: since the maximum jump is 2 steps, dp[i - 1][1] has to go to dp[i][0].

    **Step 3**: Initial States

    dp[0][0] = cost[0]

    dp[0][1] = 0

    **Step 4**: The order of Iteration: only one iteration over index from 1 to n - 1 sequentially.

    **Step 5**: Validation is omitted.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 31, 2026 |

---

[Back to Day 28 Dynamic Programming I](index.md) · [All Algorithm notes](../index.md)
