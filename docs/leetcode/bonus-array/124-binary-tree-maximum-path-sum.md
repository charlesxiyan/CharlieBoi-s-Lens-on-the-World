---
hide:
  - toc
---

# 124. Binary Tree Maximum Path Sum

*Edited by Charles*

**Hard · Python · Binary Search**

[Original problem](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

!!! tip "Core idea"

    Given the `root` of a binary tree, return the **maximum path sum** of any non-empty path.

    A path is a sequence of nodes where each pair of adjacent nodes has an edge connecting them.

## Python solution

```python
class Solution:
    def maxPathSum(self, root: Optional[TreeNode]) -> int:
        self.res = float('-inf')

        def dfs(node):
            if not node:
                return 0

            left = max(0, dfs(node.left))
            right = max(0, dfs(node.right))

            self.res = max(
                self.res,
                node.val + left + right
            )

            return node.val + max(left, right)

        dfs(root)

        return self.res
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Problem

    Given the `root` of a binary tree, return the **maximum path sum** of any non-empty path.

    A path is a sequence of nodes where each pair of adjacent nodes has an edge connecting them.

    A node can appear in the path at most once.

    The path does **not** need to pass through the root.

    ## Example

    ```text
            -10
            / \
           9   20
              /  \
             15   7
    ```

    The maximum path is:

    ```text
    15 → 20 → 7
    ```

    Therefore:

    ```text
    15 + 20 + 7 = 42
    ```

    Output:

    ```text
    42
    ```

    The key idea is to distinguish between two different values:

    1. The **complete path** through the current node, which is used to update the global maximum.
    2. The **one-sided path** starting from the current node, which is returned to its parent.

    This distinction is necessary because a valid path cannot branch.

    Define:

    ```python
    dfs(node)
    ```

    as:

    > The maximum path sum starting from `node` and going downward along only **one branch**.
    >

    Suppose:

    ```text
            20
           /  \
         15    7
    ```

    From `20`, there are two possible paths that can be returned upward:

    ```text
    20 → 15
    ```

    or:

    ```text
    20 → 7
    ```

    We choose the larger one:

    ```text
    20 + max(15, 7)
    = 35
    ```

    Therefore:

    ```python
    return node.val + max(left, right)
    ```

    Suppose `20` has a parent:

    ```text
              10
               |
              20
             /  \
           15    7
    ```

    If `20` returned both sides:

    ```text
    15 → 20 → 7
    ```

    and then the parent `10` tried to connect to it, we would get:

    ```text
              10
               |
              20
             /  \
           15    7
    ```

    This is a **Y-shaped structure**, not a path.

    A valid path cannot branch.

    Therefore, when returning information to the parent, the current node can choose only:

    ```text
    node → left
    ```

    or:

    ```text
    node → right
    ```

    So:

    ```python
    return node.val + max(left, right)
    ```

    Although we cannot return both sides to the parent, the current node itself can be the **highest point of a complete path**.

    For example:

    ```text
            20
           /  \
         15    7
    ```

    A valid complete path is:

    ```text
    15 → 20 → 7
    ```

    Therefore, at the current node, we calculate:

    ```python
    node.val + left + right
    ```

    For this example:

    ```text
    20 + 15 + 7 = 42
    ```

    This path cannot be extended upward anymore, but it may be the answer to the entire problem.

    Therefore, we use it to update the global maximum:

    ```python
    self.res = max(
        self.res,
        node.val + left + right
    )
    ```

    This is the most important distinction in this problem.

    ## Complete Path

    Used to update the final answer:

    ```python
    node.val + left + right
    ```

    Graphically:

    ```text
            node
           /    \
        left    right
    ```

    or:

    ```text
    left → node → right
    ```

    Both sides can be used because the path ends at the current subtree.

    ## Returned Path

    Used by the parent:

    ```python
    node.val + max(left, right)
    ```

    Graphically:

    ```text
            node
           /
        left
    ```

    or:

    ```text
            node
              \
              right
    ```

    Only one side can be selected because the parent may continue the path upward.

    ## Comparison

    ```text
    Complete path:

    left → node → right

    node.val + left + right

    Used to:
    Update global answer

    Returned path:

    node → left
          OR
    node → right

    node.val + max(left, right)

    Used to:
    Return to parent
    ```

    A simple way to remember:

    ```text
    Update answer → both sides

    Return upward → one side
    ```

    At every node, we need to know:

    ```text
    maximum contribution from left subtree
    maximum contribution from right subtree
    ```

    before we can calculate the answer for the current node.

    Therefore, the processing order is:

    ```text
    Left
     ↓
    Right
     ↓
    Current Node
    ```

    which is exactly **Postorder Traversal**.

    ```python
    left = dfs(node.left)
    right = dfs(node.right)

    # process current node
    ```

    Suppose:

    ```text
           5
          /
        -10
    ```

    If we include the left subtree:

    ```text
    5 + (-10) = -5
    ```

    But simply taking:

    ```text
    5
    ```

    is better.

    Therefore, if a subtree contributes a negative value, we should ignore it.

    We write:

    ```python
    left = max(0, dfs(node.left))
    right = max(0, dfs(node.right))
    ```

    This means:

    ```text
    positive contribution → use it

    negative contribution → treat it as 0
    ```

    In other words:

    ```text
    If adding a subtree makes the path worse,
    do not include that subtree.
    ```

    We use:

    ```python
    self.res = float('-inf')
    ```

    instead of:

    ```python
    self.res = 0
    ```

    because all node values could be negative.

    For example:

    ```text
        -3
    ```

    The correct answer is:

    ```text
    -3
    ```

    If we initialize:

    ```python
    self.res = 0
    ```

    then the result would incorrectly remain:

    ```text
    0
    ```

    Therefore:

    ```python
    self.res = float('-inf')
    ```

    allows negative answers.

    Consider:

    ```text
            -10
            / \
           9   20
              /  \
             15   7
    ```

    ### Node `9`

    Both children are empty:

    ```text
    left = 0
    right = 0
    ```

    Complete path:

    ```text
    9 + 0 + 0 = 9
    ```

    Update:

    ```text
    res = 9
    ```

    Return:

    ```text
    9 + max(0, 0) = 9
    ```

    ### Node `15`

    Complete path:

    ```text
    15
    ```

    Return:

    ```text
    15
    ```

    ### Node `7`

    Complete path:

    ```text
    7
    ```

    Return:

    ```text
    7
    ```

    ### Node `20`

    Now:

    ```text
    left = 15
    right = 7
    ```

    Complete path:

    ```text
    15 → 20 → 7
    ```

    Sum:

    ```text
    15 + 20 + 7 = 42
    ```

    Update:

    ```text
    res = 42
    ```

    But the value returned to `-10` can only use one side:

    ```text
    20 + max(15, 7)
    = 35
    ```

    So:

    ```python
    return 35
    ```

    ### Node `10`

    We receive:

    ```text
    left = 9
    right = 35
    ```

    Complete path through `-10`:

    ```text
    9 + (-10) + 35
    = 34
    ```

    Compare with the existing answer:

    ```text
    max(42, 34) = 42
    ```

    Therefore:

    ```text
    Final Answer = 42
    ```

    For every node:

    ```text
                node
               /    \
            left    right

              ↓ DFS ↓

    left = max(0, dfs(node.left))
    right = max(0, dfs(node.right))

              ↓

    Complete path through node:

    left + node.val + right

              ↓

    Update global answer:

    self.res = max(
        self.res,
        node.val + left + right
    )

              ↓

    Return a one-sided path:

    node.val + max(left, right)

              ↓

            Parent
    ```

    There are only two formulas that need to be remembered.

    ### Formula 1 — Update Global Answer

    ```python
    self.res = max(
        self.res,
        node.val + left + right
    )
    ```

    Meaning:

    ```text
    left → node → right
    ```

    Both sides can be included.

    ### Formula 2 — Return to Parent

    ```python
    return node.val + max(left, right)
    ```

    Meaning:

    ```text
    node → left
    ```

    or:

    ```text
    node → right
    ```

    Only one side can be included.

    ## Time Complexity

    Each node is visited exactly once:

    ```text
    O(n)
    ```

    where `n` is the number of nodes.

    ## Space Complexity

    The recursion stack depends on the height of the tree:

    ```text
    O(h)
    ```

    For a balanced tree:

    ```text
    O(log n)
    ```

    For a completely skewed tree:

    ```text
    O(n)
    ```



    The core of LeetCode 124 is understanding that the value used to update the answer and the value returned to the parent are different.

    ```text
    Update global answer:

    left + node + right

    → both sides can be used
    → complete path
    ```

    while:

    ```text
    Return to parent:

    node + max(left, right)

    → only one side can be used
    → path must not branch
    ```

    Remember:

    ```text
    Update answer → both sides

    Return upward → one side
    ```

    That is the key idea behind LeetCode 124.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | September 9, 2026 |

---

[Back to Bonus Array](index.md) · [All LeetCode notes](../index.md)
