---
hide:
  - toc
---

# 437. Path Sum III

*Edited by Charles*

**Medium · Python · Binary Tree**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/path-sum-iii/description/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        Given the `root` of a binary tree and an integer `targetSum`, return the number of paths where the sum of the values along the path equals `targetSum`.

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Problem

    Given the `root` of a binary tree and an integer `targetSum`, return the number of paths where the sum of the values along the path equals `targetSum`.

    Important requirements:

    - The path does **not** need to start from the root.
    - The path does **not** need to end at a leaf.
    - The path must move **downward**, from parent to child.

    ## Example

    ```text
              10
             /  \
            5   -3
           / \    \
          3   2    11
         / \   \
        3  -2   1

    targetSum = 8
    ```

    There are three valid paths:

    ```text
    5 → 3

    5 → 2 → 1

    -3 → 11
    ```

    Therefore:

    ```text
    Output = 3
    ```

    The key difficulty is:

    > A valid path can start from **any node**.
    >

    Therefore, we divide the problem into two parts:

    ```text
    pathSum()
        ↓
    Choose every node as a possible starting point

    backtrack()
        ↓
    Once the starting point is fixed,
    search all downward paths starting from it
    ```

    So we use two recursive functions with different responsibilities.

    Define:

    ```python
    backtrack(node, targetSum)
    ```

    as:

    > The number of valid paths that **start exactly from `node`** and go downward with sum equal to `targetSum`.
    >

    For example:

    ```text
            5
           / \
          3   2
               \
                1
    ```

    Suppose:

    ```text
    targetSum = 8
    ```

    If we call:

    ```python
    backtrack(5, 8)
    ```

    we only consider paths starting from `5`:

    ```text
    5

    5 → 3       ✓ 8

    5 → 2

    5 → 2 → 1   ✓ 8
    ```

    Therefore:

    ```text
    backtrack(5, 8) = 2
    ```

    Inside `backtrack()`:

    ```python
    if node.val == targetSum:
        count += 1
    ```

    Why?

    Because `targetSum` represents the **remaining sum we still need**.

    If:

    ```text
    node.val == targetSum
    ```

    then the path from the original starting node to the current node has exactly the required sum.

    Therefore, we find one valid path:

    ```python
    count += 1
    ```

    After using `node.val`, the remaining target becomes:

    ```python
    targetSum - node.val
    ```

    So we recursively search both children:

    ```python
    self.backtrack(node.left, targetSum - node.val)

    self.backtrack(node.right, targetSum - node.val)
    ```

    Therefore:

    ```python
    return (
        count
        + self.backtrack(node.left, targetSum - node.val)
        + self.backtrack(node.right, targetSum - node.val)
    )
    ```

    This means:

    ```text
    number of valid paths
    =
    path ending at current node
    +
    valid paths continuing into left subtree
    +
    valid paths continuing into right subtree
    ```

    `backtrack()` has one limitation:

    > It only searches paths starting from the node passed into it.
    >

    For example:

    ```text
            10
           /
          5
         /
        3
       /
      2
    ```

    Suppose:

    ```text
    targetSum = 5
    ```

    There is a valid path:

    ```text
    3 → 2 = 5
    ```

    If we only call:

    ```python
    backtrack(10, 5)
    ```

    we only consider:

    ```text
    10
    10 → 5
    10 → 5 → 3
    10 → 5 → 3 → 2
    ```

    We will never consider:

    ```text
    3 → 2
    ```

    because its starting point is `3`, not `10`.

    Therefore, we need another recursion to let **every node become a starting point**.

    That is the purpose of:

    ```python
    pathSum()
    ```

    For the current `root`:

    ```python
    self.backtrack(root, targetSum)
    ```

    counts all valid paths starting exactly from `root`.

    But paths can also start somewhere inside the left subtree:

    ```python
    self.pathSum(root.left, targetSum)
    ```

    or somewhere inside the right subtree:

    ```python
    self.pathSum(root.right, targetSum)
    ```

    Therefore:

    ```python
    return (
        self.backtrack(root, targetSum)
        + self.pathSum(root.left, targetSum)
        + self.pathSum(root.right, targetSum)
    )
    ```

    This is the most important formula of this solution.

    This distinction is extremely important.

    Suppose:

    ```text
            10
           /
          5
         /
        3
       /
      2
    ```

    If we write:

    ```python
    self.backtrack(root.left, targetSum)
    ```

    then we only consider paths starting exactly from `5`:

    ```text
    5
    5 → 3
    5 → 3 → 2
    ```

    We still do **not** consider `3` as a new independent starting point.

    But:

    ```python
    self.pathSum(root.left, targetSum)
    ```

    means:

    ```text
    Consider 5 as a starting point
    +
    Consider every node inside the subtree as a starting point
    ```

    So eventually:

    ```text
    5
    3
    2
    ```

    will all become starting nodes.

    Therefore:

    ```text
    backtrack(node)
    → fixed starting point
    ```

    while:

    ```text
    pathSum(node)
    → enumerate all possible starting points
    ```

    This solution can be understood as two levels of DFS.

    ## Outer DFS — `pathSum()`

    Purpose:

    ```text
    Enumerate every node as a starting point
    ```

    Conceptually:

    ```text
                 root
                /    \
             node    node
             /         \
          node         node

           ↓

    Try root as start
    Try every node in left subtree as start
    Try every node in right subtree as start
    ```

    ## Inner DFS — `backtrack()`

    Purpose:

    ```text
    Fix one starting point
    and explore every downward path from it
    ```

    For example:

    ```text
            start
            /   \
           A     B
          / \     \
         C   D     E
    ```

    `backtrack(start)` explores:

    ```text
    start

    start → A
    start → A → C
    start → A → D

    start → B
    start → B → E
    ```

    It never moves upward.

    Consider:

    ```text
          5
         / \
        3   2
             \
              1

    targetSum = 8
    ```

    First:

    ```python
    pathSum(5, 8)
    ```

    ### Starting from `5`

    Call:

    ```python
    backtrack(5, 8)
    ```

    At `5`:

    ```text
    remaining target = 8
    5 != 8
    ```

    Continue with:

    ```text
    remaining target = 8 - 5 = 3
    ```

    Go left to `3`:

    ```text
    3 == 3
    ```

    Found:

    ```text
    5 → 3
    ```

    Go right to `2`:

    ```text
    2 != 3
    ```

    Remaining:

    ```text
    3 - 2 = 1
    ```

    Go to `1`:

    ```text
    1 == 1
    ```

    Found:

    ```text
    5 → 2 → 1
    ```

    Therefore:

    ```text
    backtrack(5, 8) = 2
    ```

    Then `pathSum()` recursively considers:

    ```text
    3 as a new starting point
    2 as a new starting point
    1 as a new starting point
    ```

    This guarantees that every possible downward path is considered.

    In a traditional backtracking problem, we often write:

    ```python
    path.append(...)
    backtrack(...)
    path.pop()
    ```

    But this problem only asks for:

    ```text
    number of valid paths
    ```

    It does not ask us to return the actual paths.

    Therefore, we do not need to maintain a `path` list.

    Instead, we keep track of the remaining target:

    ```python
    targetSum - node.val
    ```

    Also, `targetSum` is an integer.

    Each recursive call receives its own integer value, so we do not need to manually restore it with a `pop()` operation.

    ```text
    pathSum(root)
    │
    ├── backtrack(root)
    │      │
    │      └── Count all valid paths
    │          STARTING from root
    │
    ├── pathSum(root.left)
    │      │
    │      └── Let every node in the left subtree
    │          become a new starting point
    │
    └── pathSum(root.right)
           │
           └── Let every node in the right subtree
               become a new starting point
    ```

    So:

    ```text
    pathSum
    =
    enumerate starting points
    ```

    and:

    ```text
    backtrack
    =
    explore paths from one fixed starting point
    ```

    ## Outer Recursion

    ```python
    pathSum(root)
    =
    backtrack(root)
    + pathSum(root.left)
    + pathSum(root.right)
    ```

    Meaning:

    ```text
    paths starting at root
    +
    paths starting somewhere in left subtree
    +
    paths starting somewhere in right subtree
    ```

    ## Inner Recursion

    ```python
    backtrack(node, target)
    =
    current valid path
    + backtrack(node.left, target - node.val)
    + backtrack(node.right, target - node.val)
    ```

    Meaning:

    ```text
    Check whether the path ends here
    +
    continue toward left child
    +
    continue toward right child
    ```

    ## Time Complexity

    In the worst case:

    ```text
    O(n²)
    ```

    Why?

    `pathSum()` may use every node as a starting point.

    For every starting point, `backtrack()` may visit many nodes below it.

    For a skewed tree:

    ```text
    1
     \
      2
       \
        3
         \
          4
           \
           ...
    ```

    we may perform approximately:

    ```text
    n + (n - 1) + (n - 2) + ... + 1
    ```

    operations.

    Therefore:

    ```text
    O(n²)
    ```

    ## Space Complexity

    The recursion depth depends on the height of the tree:

    ```text
    O(h)
    ```

    Worst case for a skewed tree:

    ```text
    O(n)
    ```



    The most important idea is to separate two responsibilities:

    ```text
    pathSum()
        ↓
    Where does the path start?
        ↓
    Enumerate every node as a possible starting point
    ```

    and:

    ```text
    backtrack()
        ↓
    After fixing the starting point,
    where can the path end?
        ↓
    Explore all downward paths
    ```

    So remember:

    ```text
    pathSum = enumerate starting points

    backtrack = explore paths from one starting point
    ```

    And the key code is:

    ```python
    return (
        self.backtrack(root, targetSum)
        + self.pathSum(root.left, targetSum)
        + self.pathSum(root.right, targetSum)
    )
    ```

    Do **not** replace the last two calls with:

    ```python
    self.backtrack(root.left, targetSum)
    self.backtrack(root.right, targetSum)
    ```

    because that would only make the immediate children new starting points and would miss paths starting from deeper nodes.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | September 14, 2026 |

## Python solution

```python
class Solution:
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> int:
        if not root:
            return 0

        return (
            self.backtrack(root, targetSum)
            + self.pathSum(root.left, targetSum)
            + self.pathSum(root.right, targetSum)
        )

    def backtrack(self, node, targetSum):
        if not node:
            return 0

        count = 0

        if node.val == targetSum:
            count += 1

        return (
            count
            + self.backtrack(node.left, targetSum - node.val)
            + self.backtrack(node.right, targetSum - node.val)
        )
```

---

[Back to Bonus Binary Tree](index.md) · [All Algorithm notes](../index.md)
