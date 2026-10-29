---
hide:
  - toc
---

# 121. Best Time to Buy and Sell Stock

*Edited by Charles*

**Easy · Python · Greedy**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        You are given an array `prices` where:

        is the price of a given stock on day `i`.

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Problem

    You are given an array `prices` where:

    ```text
    prices[i]
    ```

    is the price of a given stock on day `i`.

    You want to maximize your profit by choosing:

    ```text
    one day to buy
    +
    one later day to sell
    ```

    You may complete **at most one transaction**.

    Return the maximum profit.

    If no profit is possible, return:

    ```text
    0
    ```

    ## Example

    ```text
    Input:
    prices = [7, 1, 5, 3, 6, 4]

    Output:
    5
    ```

    The best transaction is:

    ```text
    Buy at 1
    Sell at 6

    Profit = 6 - 1 = 5
    ```

    LeetCode 121 allows only:

    ```text
    ONE transaction
    ```

    That means:

    ```text
    buy once
    +
    sell once
    ```

    We cannot do:

    ```text
    buy → sell → buy → sell
    ```

    multiple times.

    A tempting idea is to calculate the price difference between every two consecutive days:

    ```python
    profits = [
        prices[i] - prices[i - 1]
        for i in range(1, len(prices))
    ]
    ```

    Then add all positive differences:

    ```python
    res = 0

    for profit in profits:
        res += profit if profit > 0 else 0
    ```

    For:

    ```text
    prices = [7, 1, 5, 3, 6, 4]
    ```

    we get:

    ```text
    prices:

    7    1    5    3    6    4
     \  / \  / \  / \  / \  /
     -6    4   -2    3   -2
    ```

    Therefore:

    ```text
    profits = [-6, 4, -2, 3, -2]
    ```

    Adding all positive differences gives:

    ```text
    4 + 3 = 7
    ```

    But this corresponds to:

    ```text
    Buy at 1 → Sell at 5
    Profit = 4

    Buy at 3 → Sell at 6
    Profit = 3

    Total = 7
    ```

    This requires **two transactions**.

    Therefore, this solves:

    ```text
    LeetCode 122 — Best Time to Buy and Sell Stock II
    ```

    rather than LeetCode 121.

    For LeetCode 121, we can think about the problem from the perspective of the **selling day**.

    Suppose today we sell the stock.

    Then the maximum profit we can make is:

    ```text
    today's price
    -
    lowest price before today
    ```

    Therefore, while traversing the array, we only need to maintain:

    ```text
    minPrice
    =
    minimum stock price we have seen so far
    ```

    and:

    ```text
    res
    =
    maximum profit we have found so far
    ```

    For every price:

    ```python
    price
    ```

    assume:

    ```text
    we sell today
    ```

    Then:

    ```python
    profit = price - minPrice
    ```

    Update the maximum profit:

    ```python
    res = max(res, price - minPrice)
    ```

    Then update the minimum price:

    ```python
    minPrice = min(minPrice, price)
    ```

    Consider:

    ```text
    prices = [7, 1, 5, 3, 6, 4]
    ```

    Initialize:

    ```text
    minPrice = 7
    res = 0
    ```

    ## Day 1 — Price = 1

    If we sell today:

    ```text
    profit = 1 - 7 = -6
    ```

    So:

    ```text
    res = max(0, -6)
        = 0
    ```

    Update minimum price:

    ```text
    minPrice = min(7, 1)
             = 1
    ```

    Current state:

    ```text
    minPrice = 1
    res = 0
    ```

    ## Day 2 — Price = 5

    If we sell today:

    ```text
    profit = 5 - 1
           = 4
    ```

    Therefore:

    ```text
    res = max(0, 4)
        = 4
    ```

    Minimum price remains:

    ```text
    minPrice = 1
    ```

    ## Day 3 — Price = 3

    If we sell today:

    ```text
    profit = 3 - 1
           = 2
    ```

    Therefore:

    ```text
    res = max(4, 2)
        = 4
    ```

    Minimum price:

    ```text
    minPrice = 1
    ```

    ## Day 4 — Price = 6

    If we sell today:

    ```text
    profit = 6 - 1
           = 5
    ```

    Therefore:

    ```text
    res = max(4, 5)
        = 5
    ```

    ## Day 5 — Price = 4

    If we sell today:

    ```text
    profit = 4 - 1
           = 3
    ```

    Therefore:

    ```text
    res = max(5, 3)
        = 5
    ```

    Final answer:

    ```text
    5
    ```

    ```text
    prices = [7, 1, 5, 3, 6, 4]

                 Sell
                   ↓
    7    1    5    3    6    4
         ↑
        Buy

    Buy  = 1
    Sell = 6

    Profit = 6 - 1 = 5
    ```

    During the traversal:

    ```text
    price      minPrice      current profit      res

      7            7               -              0
      1            1              -6              0
      5            1               4              4
      3            1               2              4
      6            1               5              5
      4            1               3              5
    ```

    This is an important point.

    When we are at:

    ```python
    prices[i]
    ```

    `minPrice` only contains the minimum price we have seen **before or at the current position**.

    Therefore, when calculating:

    ```python
    prices[i] - minPrice
    ```

    the buying day cannot occur after the selling day.

    We are always doing:

    ```text
    previous minimum price → current selling price
    ```

    so the time ordering is automatically satisfied.

    For example:

    ```text
    prices = [7, 6, 4, 3, 1]
    ```

    There is no profitable transaction.

    During the traversal:

    ```text
    7 → 6 → 4 → 3 → 1
    ```

    every possible profit is negative.

    Because we initialize:

    ```python
    res = 0
    ```

    the result remains:

    ```text
    0
    ```

    which means:

    ```text
    Do not make a transaction.
    ```

    There is another interesting way to understand LeetCode 121.

    Calculate the daily price changes:

    ```python
    profits = [
        prices[i] - prices[i - 1]
        for i in range(1, len(prices))
    ]
    ```

    For:

    ```text
    prices = [7, 1, 5, 3, 6, 4]
    ```

    we get:

    ```text
    profits = [-6, 4, -2, 3, -2]
    ```

    Suppose we buy at price `1` and sell at price `6`.

    The profit is:

    ```text
    6 - 1 = 5
    ```

    But we can also write it as:

    ```text
    (5 - 1)
    +
    (3 - 5)
    +
    (6 - 3)

    = 4 - 2 + 3

    = 5
    ```

    Therefore, one stock transaction corresponds to choosing a **continuous segment** of the daily profit array.

    So LeetCode 121 can also be transformed into:

    ```text
    Find the maximum subarray sum of the daily price differences.
    ```

    This is exactly:

    ```text
    Kadane's Algorithm
    ```

    or the idea behind:

    ```text
    LeetCode 53 — Maximum Subarray
    ```

    Because:

    ```python
    sum(profit for profit in profits if profit > 0)
    ```

    allows us to skip negative days.

    For:

    ```text
    profits = [4, -2, 3]
    ```

    adding only positive values gives:

    ```text
    4 + 3 = 7
    ```

    But one transaction cannot skip the middle price drop.

    If we buy before the `+4` and sell after the `+3`, we must also experience:

    ```text
    -2
    ```

    Therefore:

    ```text
    4 - 2 + 3 = 5
    ```

    This is why the daily changes must form a **continuous subarray**.

    Using the original `profits` idea, we can write:

    ```python
    class Solution:
        def maxProfit(self, prices: List[int]) -> int:
            profits = [
                prices[i] - prices[i - 1]
                for i in range(1, len(prices))
            ]

            res = 0
            cur = 0

            for profit in profits:
                cur = max(0, cur + profit)
                res = max(res, cur)

            return res
    ```

    Here:

    ```text
    cur
    =
    maximum sum of a continuous daily-profit subarray
    ending at the current position
    ```

    This correctly enforces the one-transaction constraint.

    The direct stock-price approach is simpler:



    ## Time Complexity

    We traverse `prices` once:

    ```text
    O(n)
    ```

    ## Space Complexity

    We only maintain:

    ```text
    minPrice
    res
    ```

    Therefore:

    ```text
    O(1)
    ```

    LeetCode 121 allows:

    ```text
    at most ONE transaction
    ```

    Therefore:

    ```text
    buy once → sell once
    ```

    For every day, assume we sell today:

    ```text
    profit
    =
    today's price
    -
    minimum price seen so far
    ```

    So we maintain:

    ```python
    minPrice = min(minPrice, price)

    res = max(res, price - minPrice)
    ```

    The core idea is:

    ```text
    For each selling day,
    find the cheapest buying price before it.
    ```

    Do not use:

    ```python
    sum(all positive daily changes)
    ```

    because that represents multiple transactions and belongs to:

    ```text
    LeetCode 122
    ```

    A useful connection to remember is:

    ```text
    LC121
        ↓
    daily price differences
        ↓
    maximum CONTINUOUS sum
        ↓
    Kadane's Algorithm / LC53
    ```

    while:

    ```text
    LC122
        ↓
    daily price differences
        ↓
    sum ALL positive differences
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 5 |
    | Mastery | ❚❚❚❚❚ |
    | The Latest Attempt | September 18, 2026 |

## Python solution

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        minPrice = prices[0]
        res = 0

        for price in prices[1:]:
            res = max(res, price - minPrice)
            minPrice = min(minPrice, price)

        return res
```

---

[Back to Bonus Greedy Algorithm](index.md) · [All Algorithm notes](../index.md)
