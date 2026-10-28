---
hide:
  - toc
---

# 0/1 Knapsack Problem

*Edited by Charles*

**Medium · Python · Dynamic Programming**

!!! tip "Core idea"

    Let `dp[j]` be the best value with capacity `j`. Process each item once and update capacities in descending order so the same item cannot be reused.

## Python solution

```python
def zero_one_knapsack(size, w, v):
    dp = [0] * (size + 1)
    for weight, value in zip(w, v):
        for capacity in range(size, weight - 1, -1):
            dp[capacity] = max(dp[capacity], dp[capacity - weight] + value)
    return dp[-1]
```

!!! note "Import correction"

    The main implementation uses the original rolling-array approach, with explicit parameters instead of undefined default variables. Earlier drafts are retained below.

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    **Problem:**

    There is a knapsack of size **`sum`** . Given **`N`** items, with the weight of the **`i`** -th item **`w[i]`** and value **`v[i]`**, where **`0 <= i < N`** , and there is only one item for each **`i`**. What is the largest value of the combination of items that can be stuffed into the knapsack? See the example below.

    | Item | Weight | Value |
    | --- | --- | --- |
    | Item 0 | 0 | 0 |
    | Item 1 | 1 | 10 |
    | Item 2 | 2 | 20 |
    | Item 3 | 3 | 30 |

    **Solution:**

    1. Define the 2D array dp[i][j], **`0 <= i <= N, 0 <= j <= sum`**. The interpretation of dp[i][j]: given the size of the knapsack j, and that we are only allowed to select items from 0 to i, the maximum value in all combinations is dp[i][j].
    2. Initialization: dp[0][j] = 0, j > 1, since intuitively there is no way to stuff the knapsack if we do not select anything. Here `dp[0][0] = 0` too: this table stores maximum value, not the number of ways.


        | Item\Size | 0 | 1 | 2 | 3 | 4 |
        | --- | --- | --- | --- | --- | --- |
        | 0 | 0 | 0 | 0 | 0 | 0 |
        | 1 | 0 |  |  |  |  |
        | 2 | 0 |  |  |  |  |
        | 3 | 0 |  |  |  |  |
    3. Formula:

        $$
        dp[i][j] = \begin{cases}
        dp[i - 1][j],& \; \text{if $w[i] > sum$;}\\
        max\{dp[i - 1][j], dp[i - 1][j - w[i]] + v[i]\},& \; \text{if $w[i] \leq sum$.}
        \end{cases}
        $$

        Interpretation:

        Consider the brute force. For each item, there are only two choices for it, in the knapsack or not in the knapsack. The brute force is a backtracking algorithm. In this example, there are 8 combinations to be iterated for the brute force.

        However, in DP, the core idea of the recursive formula is

        1. at dp[i][j], we only consider the **item i**.
        2. If the size **sum = j** is not less than **w[i]**, there are two sources for dp[i][j]:
            1. the item i is not in the knapsack, then the problem is converted to **dp[i - 1][j]**.
            2. the item i is in the knapsack, then the problem is converted to **v[i] + dp[i - 1][j - w[i]]**, considering the conditional probability (the expression must have v[i]).
        3. If the size **sum = j** is less than **w[i]**, that means item i cannot be stuffed into the knapsack. Then the problem is converted into **dp[i - 1][j]** again.

        See the hand simulation of the example below.

        | Item\Size | 0 | 1 | 2 | 3 | 4 |
        | --- | --- | --- | --- | --- | --- |
        | 0 | 0 | 0 | 0 | 0 | 0 |
        | 1 | 0 | 10 | 10 | 10 | 10 |
        | 2 | 0 | 10 | 20 | 30 | 30 |
        | 3 | 0 | 10 | 20 | 30 | 40 |

    ```python
    def zero_one_knapsack(size = bagsize, w = weight, v = value):
    		l = len(w)
    		dp = [[0] * (size + 1) for _ in range(1 + l)]

    		for i in range(1: l + 1):
    				for j in range(1: size + 1):
    						if j >= w[i]:
    								dp[i][j] = max(dp[i - 1][j], v[i] + dp[i - 1][j - w[i]])
    						else:
    								dp[i][j] = dp[i - 1][j]

    		return dp[-1][-1]
    ```

    For the order of iteration, this method with 2-dimensional array does strictly require whether it should be in the ascending or descending order. Hence, the code can be:

    ```python
    def zero_one_knapsack(size = bagsize, w = weight, v = value):
    		l = len(w)
    		dp = [[0] * (size + 1) for _ in range(1 + l)]

    		for i in range(1: l + 1):
    				for j in range(size, 0, -1):
    						if j >= w[i]:
    								dp[i][j] = max(dp[i - 1][j], v[i] + dp[i - 1][j - w[i]])
    						else:
    								dp[i][j] = dp[i - 1][j]

    		# for i in range(1: l + 1):
    		# 		for j in range(size, w[i] - 1, -1):
    		# 				dp[i][j] = max(dp[i - 1][j], v[i] + dp[i - 1][j - w[i]])

    		return dp[-1][-1]
    ```

    **Modification:**

    There is another way to modify the algorithm by maintaining an one-dimensional array (dynamically rolling array), instead of a two-dimensional array.

    1. Define the dp array:

        dp[j] refers to the maximum value of the knapsack, given the current bagsize is j.

    2. Formula:

        dp[j] = max(dp[j], dp[j - w[i]] + v[i]),

        where the maximum is attained by comparing the current dp[i] (attained from the last iteration, i.e. item i not included) and dp[j - w[i]] + v[i] (including the item i, and updating dp[i] based on dp[i - w[i]] from the last iteration).

        There will be two iterations: i and j. Iteration i refers to a traversal through all weight/value arrays. Iteration j refers to a traversal of all possible cases of bagsizes for whether to include item i or not.

    3. Initialization

        All 0.

    4. Iteration

        However, the only notion is the order of the iteration. See the example below. Note dp[0] = 0.

        | Item | Weight | Value |
        | --- | --- | --- |
        | Item 0 | 0 | 0 |
        | Item 1 | 1 | 10 |
        | Item 2 | 2 | 20 |
        | Item 3 | 3 | 30 |

        Ascending order:

        i = 1, j = 1: dp[1] = max(dp[1 - w[1]] + v[1], dp[1]) = max(0 + 10, 0) = 10

        i = 1, j = 2: dp[2] = max(dp[2 - w[1]] + v[1], dp[2]) = max(10 + 10, 0) = 20

        **Problem:**

        It is impossible for dp[2] = 20, only given the item 1. This is the weakness of the ascending order, since item 1 has been added twice (dp[1] is updated first and dp[2] uses the new dp[1] to update itself.)

        Hence, we need to makes sure dp[2] updates earlier than dp[1], i.e. iteration in the descending order.

        Descending order:

        i = 1, j = 2: dp[2] = max(dp[2 - w[1]] + v[1], dp[2]) = max(0 + 10, 0) = 10

        i = 1, j = 1: dp[1] = max(dp[1 - w[1]] + v[1], dp[1]) = max(0 + 10, 0) = 10

        **Note:**

        The items must be iterated first, and then the bagsize, since we compress the 2-dimensional array into a 1-dimentional array by deducing the dimension of items. We need to make sure every item has been considered in each case of bagsize first. This will influence the outcome later.

    5. Hand Simulation:

        TBC.




    Classic Knapsack Interview Problem:

    1. Use a 2-dimensional array to implement a 0-1 Knapsack algorithm
    2. If implemented, can we swap two for-loops?
    3. Use an 1-dimensional array to implement a 0-1 Knapsack algorithm
    4. If implemented, can we swap two for-loops?
    5. Why the second for-loop has to be the descending order, instead of the ascending order?
    6. Why the second for-loop in 2-dimensional array is not required to iterate in the descending order?

??? note "Original implementation draft"

    ```text
    def zero_one_knapsack(size = bagsize, w = weight, v = value):
    		l = len(w)
    		dp = [0] * (size + 1)

    		for i in range(l):
    				for j in range(size, w[i] - 1, -1):
    						dp[j] = max(dp[j - w[i]] + v[i], dp[j])

    		return dp[-1]
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | February 26, 2026 |
    | Feedback | Multiple Attempts |

---

[Back to Day 30 Dynamic Programming III](index.md) · [All LeetCode notes](../index.md)
