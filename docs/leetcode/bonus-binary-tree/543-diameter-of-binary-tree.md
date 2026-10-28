---
hide:
  - toc
---

# 543. Diameter of Binary Tree

*Edited by Charles*

**Easy · Python · Binary Tree**

[Original problem](https://leetcode.com/problems/diameter-of-binary-tree/description/)

!!! tip "Core idea"

    Given the `root` of a binary tree, return the **diameter** of the tree.

    The diameter is the length of the longest path between any two nodes in the tree.

## Python solution

```python
class Solution:
    def diameterOfBinaryTree(self, root: Optional[TreeNode]) -> int:
        self.res = 0

        self.traverse(root)

        return self.res

    def traverse(self, node):
        if not node:
            return 0

        leftDepth = self.traverse(node.left)
        rightDepth = self.traverse(node.right)

        self.res = max(
            self.res,
            leftDepth + rightDepth
        )

        return 1 + max(leftDepth, rightDepth)
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Problem

    Given the `root` of a binary tree, return the **diameter** of the tree.

    The diameter is the length of the longest path between any two nodes in the tree.

    The path does **not** necessarily need to pass through the root.

    The length of a path is measured by the **number of edges**, not the number of nodes.

    ## Example

    ```text
            1
           / \
          2   3
         / \
        4   5
    ```

    The longest path can be:

    ```text
    4 → 2 → 1 → 3
    ```

    or:

    ```text
    5 → 2 → 1 → 3
    ```

    There are `3` edges.

    Therefore:

    ```text
    Output = 3
    ```

    The key idea is to distinguish between two concepts:

    1. The **diameter passing through the current node**
    2. The **maximum depth returned to the parent**

    This is very similar to LeetCode 124 — Binary Tree Maximum Path Sum.

    For every node:

    ```text
              node
             /    \
           left   right
    ```

    we first recursively calculate:

    ```python
    leftDepth = self.traverse(node.left)
    rightDepth = self.traverse(node.right)
    ```

    Then we use these two depths to:

    ```text
    1. Update the global diameter
    2. Return the maximum depth to the parent
    ```

    Define:

    ```python
    traverse(node)
    ```

    as:

    > The maximum depth starting from `node` and going downward along one branch.
    >

    For example:

    ```text
            2
           / \
          4   5
         /
        6
    ```

    Starting from `2`, the longest downward path is:

    ```text
    2 → 4 → 6
    ```

    Therefore:

    ```text
    traverse(2) = 3
    ```

    Notice that this value represents the number of **nodes** in the downward path starting from `2`.

    This is why we return:

    ```python
    return 1 + max(leftDepth, rightDepth)
    ```

    The `1` represents the current node.

    If the current node does not exist:

    ```python
    if not node:
        return 0
    ```

    An empty subtree has depth:

    ```text
    0
    ```

    For a leaf node:

    ```text
        4
    ```

    we get:

    ```text
    leftDepth = 0
    rightDepth = 0
    ```

    Therefore:

    ```python
    return 1 + max(0, 0)
    ```

    which gives:

    ```text
    1
    ```

    So a leaf node has depth `1`.

    We recursively calculate:

    ```python
    leftDepth = self.traverse(node.left)
    rightDepth = self.traverse(node.right)
    ```

    Because the current node needs information from both children before it can calculate its own result, this is a **Postorder DFS**:

    ```text
    Left
     ↓
    Right
     ↓
    Current Node
    ```

    Suppose:

    ```text
              node
             /    \
            A      B
           /        \
          ...       ...
    ```

    The longest path passing through `node` can use:

    ```text
    deepest node in left subtree
            ↓
          ...
            ↓
           left
            ↓
           node
            ↓
          right
            ↓
           ...
            ↓
    deepest node in right subtree
    ```

    Therefore, the diameter passing through the current node is:

    ```python
    leftDepth + rightDepth
    ```

    Why don't we add `1`?

    Because the diameter is measured by the number of **edges**.

    For example:

    ```text
            1
           / \
          2   3
         /
        4
    ```

    At node `1`:

    ```text
    leftDepth = 2
    rightDepth = 1
    ```

    The path is:

    ```text
    4 → 2 → 1 → 3
    ```

    The number of edges is:

    ```text
    4 → 2    1 edge
    2 → 1    1 edge
    1 → 3    1 edge
    ```

    Total:

    ```text
    3 edges
    ```

    And:

    ```text
    leftDepth + rightDepth
    = 2 + 1
    = 3
    ```

    Therefore:

    ```python
    self.res = max(
        self.res,
        leftDepth + rightDepth
    )
    ```

    The longest path does not necessarily pass through the root.

    For example:

    ```text
                 1
                /
               2
              / \
             3   4
            /     \
           5       6
    ```

    The longest path may pass through node `2` rather than using the root as its highest point.

    Therefore, at **every node**, we calculate:

    ```python
    leftDepth + rightDepth
    ```

    and compare it with the current global maximum:

    ```python
    self.res = max(
        self.res,
        leftDepth + rightDepth
    )
    ```

    So `self.res` represents:

    > The maximum diameter found anywhere in the tree so far.
    >

    This is the most important part of the problem.

    When calculating the diameter at the current node, we can use both sides:

    ```text
    left → node → right
    ```

    So:

    ```python
    leftDepth + rightDepth
    ```

    can be used to update the global answer.

    However, when returning a depth to the parent, we cannot return both sides.

    Suppose:

    ```text
               parent
                 |
                node
               /    \
            left    right
    ```

    If `node` returned both sides to `parent`, the structure would become:

    ```text
               parent
                 |
                node
               /    \
            left    right
    ```

    This is a Y-shaped structure.

    It is **not a single path** because it branches.

    Therefore, when returning to the parent, we must choose only one side:

    ```text
    node → left
    ```

    or:

    ```text
    node → right
    ```

    We choose the deeper one:

    ```python
    return 1 + max(leftDepth, rightDepth)
    ```

    This is the core idea of LeetCode 543.

    ## Update the Diameter

    At the current node, both sides can be used:

    ```text
    left → node → right
    ```

    Therefore:

    ```python
    self.res = max(
        self.res,
        leftDepth + rightDepth
    )
    ```

    ## Return to Parent

    When returning upward, only one side can be used:

    ```text
    node → left
    ```

    or:

    ```text
    node → right
    ```

    Therefore:

    ```python
    return 1 + max(leftDepth, rightDepth)
    ```

    Consider:

    ```text
            1
           / \
          2   3
         / \
        4   5
    ```

    Initially:

    ```python
    self.res = 0
    ```

    ## Node `4`

    Node `4` is a leaf.

    ```text
    leftDepth = 0
    rightDepth = 0
    ```

    Update diameter:

    ```text
    0 + 0 = 0
    ```

    So:

    ```text
    res = 0
    ```

    Return depth:

    ```text
    1 + max(0, 0) = 1
    ```

    ## Node `5`

    Same:

    ```text
    leftDepth = 0
    rightDepth = 0
    ```

    Update:

    ```text
    res = 0
    ```

    Return:

    ```text
    1
    ```

    ## Node `2`

    Now:

    ```text
    leftDepth = 1
    rightDepth = 1
    ```

    The complete path passing through `2` is:

    ```text
    4 → 2 → 5
    ```

    Diameter:

    ```text
    leftDepth + rightDepth
    = 1 + 1
    = 2
    ```

    Update:

    ```text
    res = 2
    ```

    Return to node `1`:

    ```text
    1 + max(1, 1)
    = 2
    ```

    This represents the depth:

    ```text
    2 → 4
    ```

    or:

    ```text
    2 → 5
    ```

    ## Node `3`

    Node `3` is a leaf.

    Return:

    ```text
    1
    ```

    ## Node `1`

    Now:

    ```text
    leftDepth = 2
    rightDepth = 1
    ```

    The complete path can be:

    ```text
    4 → 2 → 1 → 3
    ```

    Diameter:

    ```text
    2 + 1 = 3
    ```

    Update:

    ```text
    res = 3
    ```

    Finally:

    ```text
    Output = 3
    ```

    For every node:

    ```text
                node
               /    \
            left    right

               ↓

    Calculate:

    leftDepth = traverse(left)
    rightDepth = traverse(right)

               ↓

    Complete path through current node:

    leftDepth + rightDepth

               ↓

    Update global diameter:

    res = max(res, leftDepth + rightDepth)

               ↓

    Return one branch to parent:

    1 + max(leftDepth, rightDepth)
    ```

    LeetCode 543 and LeetCode 124 have almost exactly the same recursive structure.

    ## LeetCode 124 — Maximum Path Sum

    Update global answer:

    ```python
    self.res = max(
        self.res,
        node.val + left + right
    )
    ```

    Return to parent:

    ```python
    return node.val + max(left, right)
    ```

    ## LeetCode 543 — Diameter

    Update global answer:

    ```python
    self.res = max(
        self.res,
        leftDepth + rightDepth
    )
    ```

    Return to parent:

    ```python
    return 1 + max(leftDepth, rightDepth)
    ```

    So both problems follow the same pattern:

    ```text
    Update global answer
    → Both left and right can be used

    Return to parent
    → Only one side can be used
    ```

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



    The most important distinction is:

    ```text
    What do we use to update the answer?

    leftDepth + rightDepth
    ```

    because the complete path can go:

    ```text
    left → node → right
    ```

    But:

    ```text
    What do we return to the parent?

    1 + max(leftDepth, rightDepth)
    ```

    because a path returned to the parent cannot branch.

    The easiest way to remember LeetCode 543 is:

    ```text
    Update answer → use both sides

    Return upward → use one side
    ```

    or simply:

    ```python
    # complete path
    self.res = max(self.res, leftDepth + rightDepth)

    # one-sided depth
    return 1 + max(leftDepth, rightDepth)
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | September 14, 2026 |

---

[Back to Bonus Binary Tree](index.md) · [All LeetCode notes](../index.md)
