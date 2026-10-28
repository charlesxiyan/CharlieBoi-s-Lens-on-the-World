---
hide:
  - toc
---

# 494. Target Sum

*Edited by Charles*

**Medium · Python · Dynamic Programming**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/target-sum/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Knapsack Solution:

    Recall [0/1 Knapsack Problem](../day-30/0-1-knapsack-problem.md) for the core idea. Left Group = the sum of numbers with + signs; Right Group = the sum of numbers with - signs.

## Python solution

```python
class Solution:
    def findTargetSumWays(self, nums: List[int], target: int) -> int:

        # addSum + minusSum = total
        # minusSum = total - addSum
        # addSum - minusSum = target


        l = len(nums)
        total = sum(nums)

        if abs(target) > total:
            return 0
        if (total + target) % 2:
            return 0

        addSum = (total + target) // 2
        dp = [0] * (addSum + 1)
        # Initialization: the racksack has max value container 0;
			  #                 the combinations of positive numbers is 1,
			  #                 i.e. addSum = [] or no positive numbers.
        dp[0] = 1

        for i in range(l):
            for j in range(addSum, nums[i] - 1, -1):
                dp[j] += dp[j - nums[i]]

        return dp[-1]
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Knapsack Solution:

    1. Recall [0/1 Knapsack Problem](../day-30/0-1-knapsack-problem.md) for the core idea.
    2. Left Group = the sum of numbers with + signs; Right Group = the sum of numbers with - signs.
    3. Left + Right = Total (fixed) → Right = Total - Left
    4. Left - Right = Target (considering signs) → Left = (Total + Target) / 2. Based on the expression, we require
        1. Left is an integer.
        2. Total > abs(target). Say, Total = 5, Target = 10.
    5. dp[i] represents the number of ways to fully stuff the knapsack with size i.
    6. Initialization: dp[0] = 1, i.e. no way [].
    7. Iteration: since all items can be used for only once, the outer loop should be knapsack size, and the inner loop should be the item values in descending order to ensure only once usage.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | February 5, 2026 |

---

[Back to Day 31 Dynamic Programming IV](index.md) · [All Algorithm notes](../index.md)
