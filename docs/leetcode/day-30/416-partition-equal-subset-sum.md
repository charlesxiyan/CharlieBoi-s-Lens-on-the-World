---
hide:
  - toc
---

# 416. Partition Equal Subset Sum

*Edited by Charles*

**Medium · Python · Dynamic Programming**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/partition-equal-subset-sum/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    This is an application of 0-1 knapsack problem. Consider the number as both the weight and the value.

## Python solution

```python
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        # 题目要求把数组分成两个子集,使两个子集的和相等
        # 等价于:能否从nums中选出若干个数,使其和恰好等于 sum(nums) / 2
        # 这是一个经典的 0/1 背包问题(能否装满容量为 target 的背包)

        s = sum(nums)
        # 如果总和是奇数,不可能平分成两个相等的整数子集,直接返回False
        if s % 2:
            return False
        s //= 2  # s 变为目标值 target,即每个子集应达到的和

        n = len(nums)

        # f[i][j] 表示:从前 i 个数中选取若干个数,是否能凑出和为 j
        # 行数 n+1:表示"前0个数"到"前n个数"共 n+1 种状态
        # 列数 s+1:表示 j 可以取 0 到 s
        f = [[False] * (s + 1) for _ in range(n + 1)]

        # 初始条件:前0个数,能凑出和为0(什么都不选,和为0),这是必然成立的
        f[0][0] = True
        # 注意 f[0][j] (j>0) 默认就是 False,因为一个数都不选凑不出正数的和

        # 外层遍历每一个数字(第i个数,对应下标i,值为x)
        for i, x in enumerate(nums):
            # 内层遍历所有可能的目标和 j
            for j in range(s + 1):
                # 状态转移方程:
                # f[i+1][j] 表示考虑了前 i+1 个数(即包含当前数x)后,能否凑出和为j
                # 有两种选择:
                #   1) 不选第i个数(下标为i,值为x): 那么能否凑出j取决于前i个数能否凑出j,即 f[i][j]
                #   2) 选第i个数: 那么需要前i个数凑出 j - x,再加上x就等于j,即 f[i][j - x]
                #      但前提是 j >= x,否则第i个数比j还大,根本没法选它凑出j

                # f[i + 1][j] =   j >= x and f[i][j - x] or f[i][j]  (等价写法,不选和选取或运算)

                if j >= x:
                    # j能容纳下x,两种情况取或:选x 或者 不选x
                    f[i+1][j] = f[i][j - x] or f[i][j]
                else:
                    # j连x都装不下,只能不选x,直接继承之前的状态
                    f[i+1][j] = f[i][j]

        # 最终答案:用全部n个数,能否凑出和为s(即target)
        return f[n][s]
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. This is an application of 0-1 knapsack problem.
    2. Consider the number as both the weight and the value.
        1. the weight ranges from 0 to sum/2.
        2. dp is the array of max values with maximum weight nums[i].
        3. the list can be divided into equal subsets if sum/2 == dp[sum/2].
    3. Tips: if the summation of nums is not even, then it is impossible to partition equal subsets.

    ```python
    class Solution:
        def canPartition(self, nums: List[int]) -> bool:
            n = len(nums)
            total = sum(nums)

            if total % 2:
                return False

            target = total // 2
            dp = [0] * (target + 1)

            for i in range(n):
                for j in range(target, nums[i] - 1, -1):
                    dp[j] = max(dp[j], dp[j - nums[i]] + nums[i])

            return dp[-1] == target
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | February 4, 2026 |

---

[Back to Day 30 Dynamic Programming III](index.md) · [All Algorithm notes](../index.md)
