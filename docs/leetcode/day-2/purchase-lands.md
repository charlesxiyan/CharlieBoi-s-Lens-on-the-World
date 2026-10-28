---
hide:
  - toc
---

# Purchase Lands

*Edited by Charles*

**Medium · Python · Array**

[Original problem](https://www.programmercarl.com/kamacoder/0044.%E5%BC%80%E5%8F%91%E5%95%86%E8%B4%AD%E4%B9%B0%E5%9C%9F%E5%9C%B0.html)

!!! tip "Core idea"

    Compute row and column totals, then scan each valid horizontal and vertical cut. If one side sums to `s` and the total is `T`, the difference is `abs(T - 2 * s)`.

## Python solution

```python
import sys


def purchase_lands(n, m, values):
    rows = [0] * n
    cols = [0] * m
    for i in range(n):
        for j in range(m):
            value = values[i * m + j]
            rows[i] += value
            cols[j] += value

    total = sum(rows)
    best = float("inf")
    for totals in (rows, cols):
        prefix = 0
        for value in totals[:-1]:  # Both regions must be nonempty.
            prefix += value
            best = min(best, abs(total - 2 * prefix))
    return best


if __name__ == "__main__":
    data = list(map(int, sys.stdin.read().split()))
    if data:
        n, m = data[:2]
        print(purchase_lands(n, m, data[2:]))
```

!!! note "Import correction"

    The main implementation repairs an incomplete source draft: input parsing, missing initialization, and cuts that would leave an empty region. The original draft is retained below.

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Question:

    A city has been divided into n*m tiles, with different weight on each tile as its value. A and B are real estate companies, hoping to purchase lands in the city.

    Now, distribute these tiles to A and B.

    However, there is a rule that only allows a vertical or horizontal partition to the whole land, and each sub-area cannot be empty.

    In order to guarantee the fairness, find a solution to ensure the values of sub-areas that A and B obtain have the minimal difference.

    Input Sample (rows):

    3

    3

    1 2 3

    2 1 3

    1 2 3

    Explanation:

    The first 2 rows are respectively n and m.

    The following n rows are weights of tiles in the land.

    Output Sample:

    0

    Explanation:

    1 2 | 3

    2 1 | 3

    1 2 | 3

    A vertical partition.

??? note "Original implementation draft"

    ```text
    import sys
    import sys.stdin.read

    def PurchaseLands()
        data = input().split()

        index = 0
        n = int(data[index])
        index += 1
        m = int(data[index])
        index += 1

        sum_value = 0
        lands = [[0] * m for _ in range(n)]
        for i in range(n):
    		    for j in range(m):
    				    weight = int(data[index])
    				    index += 1
    				    lands[i][j] = weight
    				    sum_value += weight

    		horizontal = [0] * n
    		vertical = [0] * m

    		for i in range(n):
    				for j in range(m):
    						horizontal[i] += lands[i][j]

    		for i in range(m):
    				for j in range(n):
    						vertical[i] += lands[j][i]

    		result = float('inf')

    		for i in range(n):
    				horizontalCut += horizontal[i]
    				result = min(result, abs(sum_value - 2 * horizontalCut))

    		for j in range(m):
    				verticalCut += vertical[j]
    				result = min(result, abs(sum_value - 2 * verticalCut))
    		print(result)
    		return result
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 3 |
    | Mastery | ❚❚❚ |
    | The Latest Attempt | January 3, 2026 |

---

[Back to Day 2 Array II](index.md) · [All LeetCode notes](../index.md)
