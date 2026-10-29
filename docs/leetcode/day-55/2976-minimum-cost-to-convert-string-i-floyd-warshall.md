---
hide:
  - toc
---

# 2976. Minimum Cost to Convert String I (Floyd-Warshall)

*Edited by Charles*

**Medium · Python · Graph**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/minimum-cost-to-convert-string-i/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        The key observation is that each lowercase English letter can be considered a **node in a directed weighted graph**.

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Problem

    You are given two strings `source` and `target` of the same length.

    You are also given three arrays:

    ```text
    original
    changed
    cost
    ```

    where:

    ```text
    original[i] -> changed[i]
    ```

    represents a valid character conversion with cost:

    ```text
    cost[i]
    ```

    A character may be converted multiple times through intermediate characters.

    For example:

    ```text
    a -> b, cost = 2
    b -> c, cost = 3
    ```

    Then:

    ```text
    a -> c
    ```

    can be achieved with total cost:

    ```text
    2 + 3 = 5
    ```

    The goal is to convert `source` into `target` with the minimum total cost.

    If the conversion is impossible, return `-1`.

    ## Approach: Floyd-Warshall

    The key observation is that each lowercase English letter can be considered a **node in a directed weighted graph**.

    There are only:

    ```text
    26 lowercase letters
    ```

    so the graph contains exactly 26 nodes:

    ```text
    a, b, c, ..., z
    ```

    A conversion rule:

    ```text
    original[i] -> changed[i]
    ```

    is a directed edge whose weight is:

    ```text
    cost[i]
    ```

    Therefore, the problem becomes:

    > Find the minimum conversion cost between every pair of letters.
    >

    This is an **All-Pairs Shortest Path** problem, so Floyd-Warshall is a natural solution.

    ## Define `dp`

    Create a `26 × 26` matrix:

    ```python
    dp = [[float('inf')] * 26 for _ in range(26)]
    ```

    where:

    ```text
    dp[i][j]
    ```

    represents:

    > The minimum cost to convert letter `i` into letter `j`.
    >

    For example:

    ```text
    dp[0][1] = minimum cost from 'a' to 'b'
    dp[0][2] = minimum cost from 'a' to 'c'
    dp[1][3] = minimum cost from 'b' to 'd'
    ```

    The size is `26 × 26` because the **nodes of the graph are characters**, not positions in `source` and `target`.

    ## Initialize the Diagonal

    Converting a character into itself costs nothing.

    Therefore:

    ```python
    for i in range(26):
        dp[i][i] = 0
    ```

    So:

    ```text
    a -> a = 0
    b -> b = 0
    c -> c = 0
    ...
    z -> z = 0
    ```

    All other conversions are initially impossible:

    ```text
    ∞
    ```

    ## Convert Characters to Array Indices

    We need to map:

    ```text
    'a' -> 0
    'b' -> 1
    'c' -> 2
    ...
    'z' -> 25
    ```

    We can use Python's `ord()` function:

    ```python
    ord(char) - ord('a')
    ```

    For example:

    ```text
    ord('a') - ord('a') = 97 - 97 = 0
    ord('b') - ord('a') = 98 - 97 = 1
    ord('c') - ord('a') = 99 - 97 = 2
    ```

    Therefore:

    ```python
    u = ord(org) - ord('a')
    v = ord(cgd) - ord('a')
    ```

    converts the characters into valid indices of `dp`.

    ## Add the Direct Conversion Rules

    For every conversion:

    ```text
    org -> cgd
    ```

    with cost:

    ```text
    c
    ```

    we update:

    ```python
    dp[u][v] = min(dp[u][v], c)
    ```

    We need `min()` because there may be multiple rules between the same two characters.

    For example:

    ```text
    a -> b, cost = 5
    a -> b, cost = 2
    ```

    We should keep:

    ```text
    dp[a][b] = 2
    ```

    rather than simply using whichever rule appears last.

    ## Floyd-Warshall

    Now run Floyd-Warshall:

    ```python
    for k in range(26):
        for i in range(26):
            for j in range(26):
                dp[i][j] = min(
                    dp[i][j],
                    dp[i][k] + dp[k][j]
                )
    ```

    The meaning of the transition is:

    ```text
    Current best:

    i --------> j
       dp[i][j]

    Try going through k:

    i --------> k --------> j
      dp[i][k]    dp[k][j]
    ```

    Therefore:

    ```text
    dp[i][j]
    =
    min(
        current dp[i][j],
        dp[i][k] + dp[k][j]
    )
    ```

    In other words:

    > Check whether using `k` as an intermediate character makes the conversion from `i` to `j` cheaper.
    >

    ## Example of Floyd-Warshall

    Suppose the conversion rules are:

    ```text
    a -> b = 2
    b -> c = 3
    a -> c = 10
    ```

    Initially:

    ```text
    dp[a][b] = 2
    dp[b][c] = 3
    dp[a][c] = 10
    ```

    When Floyd considers `b` as an intermediate character:

    ```text
    a -> b -> c
    ```

    the cost is:

    ```text
    2 + 3 = 5
    ```

    Since:

    ```text
    5 < 10
    ```

    we update:

    ```text
    dp[a][c] = 5
    ```

    So Floyd automatically finds cheaper conversions involving multiple intermediate characters.

    ## Convert `source` to `target`

    After Floyd-Warshall finishes:

    ```text
    dp[u][v]
    ```

    contains the minimum cost to convert any character `u` into any character `v`.

    Now iterate through `source` and `target` position by position:

    ```python
    for charS, charT in zip(source, target):
    ```

    Convert both characters into indices:

    ```python
    u = ord(charS) - ord('a')
    v = ord(charT) - ord('a')
    ```

    Then the minimum conversion cost for this position is simply:

    ```python
    dp[u][v]
    ```

    Add it to the result:

    ```python
    res += dp[u][v]
    ```

    ## Impossible Conversion

    If:

    ```python
    dp[u][v] == float('inf')
    ```

    then there is no sequence of conversion rules that can transform:

    ```text
    charS -> charT
    ```

    Therefore the entire string conversion is impossible:

    ```python
    return -1
    ```

    Otherwise, add the minimum cost to the answer.

    ## Why Can We Calculate Each Position Independently?

    Each operation changes one character.

    Changing the character at one position does not affect characters at other positions.

    Therefore:

    ```text
    source[0] -> target[0]
    source[1] -> target[1]
    source[2] -> target[2]
    ...
    ```

    can be solved independently.

    The final answer is simply:

    ```text
    minimum cost for position 0
    +
    minimum cost for position 1
    +
    minimum cost for position 2
    +
    ...
    ```

    ## Overall Process

    ```text
    original / changed / cost
                ↓
    Build a graph of 26 characters
                ↓
    Initialize dp[26][26]
                ↓
    Add direct conversion costs
                ↓
    Run Floyd-Warshall
                ↓
    Find minimum cost between every pair of characters
                ↓
    Traverse source and target
                ↓
    For each position:
    source[i] -> target[i]
                ↓
    Look up dp[u][v]
            ↙       ↘
          inf       finite
           ↓           ↓
     return -1     add to res
                       ↓
                  return res
    ```

    ## Why Floyd-Warshall?

    There are only 26 possible lowercase letters.

    Floyd-Warshall takes:

    ```text
    O(V³)
    ```

    Here:

    ```text
    V = 26
    ```

    so:

    ```text
    26³ = 17,576
    ```

    which is extremely small.

    After Floyd-Warshall, every character conversion can be answered in:

    ```text
    O(1)
    ```

    time.

    ## Complexity

    Let:

    ```text
    n = len(source)
    m = len(cost)
    ```

    ### Building the Graph

    We process every conversion rule once:

    ```text
    O(m)
    ```

    ### Floyd-Warshall

    There are 26 nodes:

    ```text
    O(26³)
    ```

    ### Processing the Strings

    We traverse `source` and `target` once:

    ```text
    O(n)
    ```

    Therefore the total time complexity is:

    ```text
    O(m + 26³ + n)
    ```

    Since `26³` is a constant, this is effectively:

    ```text
    O(m + n)
    ```

    ### Space Complexity

    The Floyd matrix is:

    ```text
    26 × 26
    ```

    Therefore:

    ```text
    O(26²)
    ```

    which is effectively:

    ```text
    O(1)
    ```

    ## Key Takeaways

    ### Graph Modeling

    ```text
    character = node

    conversion rule = directed weighted edge
    ```

    ### Floyd-Warshall

    ```text
    dp[i][j]
    = minimum cost to convert i into j
    ```

    Transition:

    ```python
    dp[i][j] = min(
        dp[i][j],
        dp[i][k] + dp[k][j]
    )
    ```

    ### Character Mapping

    ```python
    ord(char) - ord('a')
    ```

    maps:

    ```text
    a -> 0
    b -> 1
    ...
    z -> 25
    ```

    ### Overall Idea

    ```text
    26 characters
         ↓
    Floyd-Warshall
         ↓
    all-pairs minimum conversion costs
         ↓
    source[i] -> target[i]
         ↓
    look up dp and sum the costs
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | September 3, 2026 |

## Python solution

```python
class Solution:
    def minimumCost(
        self,
        source: str,
        target: str,
        original: List[str],
        changed: List[str],
        cost: List[int]
    ) -> int:

        dp = [[float('inf')] * 26 for _ in range(26)]

        for i in range(26):
            dp[i][i] = 0

        for org, cgd, c in zip(original, changed, cost):
            u = ord(org) - ord('a')
            v = ord(cgd) - ord('a')
            dp[u][v] = min(dp[u][v], c)

        # Floyd-Warshall
        for k in range(26):
            for i in range(26):
                for j in range(26):
                    dp[i][j] = min(
                        dp[i][k] + dp[k][j],
                        dp[i][j]
                    )

        res = 0

        for charS, charT in zip(source, target):
            u = ord(charS) - ord('a')
            v = ord(charT) - ord('a')

            if dp[u][v] == float('inf'):
                return -1

            res += dp[u][v]

        return res
```

---

[Back to Day 55 Graph XIII](index.md) · [All Algorithm notes](../index.md)
