---
hide:
  - toc
---

# 134. Gas Station

*Edited by Charles*

**Medium · Python · Greedy**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/gas-station/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Calculate the remaining gas in the tank if we only consider setting off from station i and arrive the next station.

    ```python
    remain = [gas[i] - cost[i] for i in range(n)]
    ```

## Python solution

```python
class Solution:
    def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:
        ans = min_s = s = 0  # s 表示油量，min_s 表示最小油量
        for i, (g, c) in enumerate(zip(gas, cost)):
            s += g - c  # 在 i 处加油，然后从 i 到 i+1
            if s < min_s:
                min_s = s  # 更新最小油量
                ans = i + 1  # 注意 s 减去 c 之后，汽车在 i+1 而不是 i
        # 循环结束后，s 即为 gas 之和去 cost 之和
        return -1 if s < 0 else ans
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. Calculate the remaining gas in the tank if we only consider setting off from station i and arrive the next station.

        ```python
        remain = [gas[i] - cost[i] for i in range(n)]
        ```

    2. The logic here is:
        1. Assume we start from station 0 and the real time remaining gas in the tank just becomes negative at station 5, i.e. out of gas on the way to 5 from 4. Note **tank = 0 at station 0**.
        2. That is to say, starting from any station in {1, 2, 3, 4} would not make us arrive at station 5.
        3. The feasible stations can only be stations after 5 (inclusive of 5).
    3. Take the following as an example.

        ```python
        gas  = [4, 3, 1, 2, 7, 4]
        cost = [1, 2, 7, 3, 2, 5]

        remain = [3, 1, -6, -1, 5, -1]
        ```

        1. Start from station 0. When i = 2, tank = 3 + 1 - 6 < 0. That is, we cannot arrive at station 3 from 0.
        2. To verify the stations on the way to the station 3,
            1. i = 1, tank = 1 - 6 < 0.
            2. i = 2, tank = -6 < 0.
        3. Note that **this trick deal with real time tank. Once the shortage of gas is found, the feasible start is searched from a new start, and the TANK IS CLEARED for searching!**

    ```python
    class Solution:
        def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:
            tank = 0
            n = len(gas)
            start = 0

            remain = [gas[i] - cost[i] for i in range(n)]
            if sum(remain) < 0:
                return -1

            for i in range(n):
                tank += remain[i]
                if tank < 0:
                    tank = 0
                    start = i + 1

            return 0 if start == n else start
    ```

    https://leetcode.cn/problems/gas-station/solutions/54278/shi-yong-tu-de-si-xiang-fen-xi-gai-wen-ti-by-cyayc/

    the slope of s depends on gas and cost. the intercept depends on the starting point

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 28, 2026 |

---

[Back to Day 25 Greedy Algorithm III](index.md) · [All Algorithm notes](../index.md)
