---
hide:
  - toc
---

# 112. Path Sum

*Edited by Charles*

**Easy · Python · Binary Tree**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/path-sum/description/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        Recursion:

        Traverse all nodes, and for each node,

    Original study notes follow, including intermediate snippets and earlier attempts.

    Recursion:

    Traverse all nodes, and for each node,

    1. if it does not have any child node, and the current node’s value is the remaining value, return True; else go to next recursion.
    2. if the recursion has been to any None node, that means the reaming value is still none-zero but the recursion already arrives at a leaf.

    ```python
    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, val=0, left=None, right=None):
    #         self.val = val
    #         self.left = left
    #         self.right = right
    class Solution:
        def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
            if root is None:
                return False

            if not root.left and not root.right and targetSum == root.val:
                return True

            return self.hasPathSum(root.left, targetSum - root.val) or self.hasPathSum(root.right, targetSum - root.val)
    ```

    Iteration:

    The idea of iteration differs from the basic iteration idea.

    1. The purpose is still to traverse all nodes and sum up the nodes on each path.
    2. Once the current node has no child node and the summation from the tuple is the targetSum, return True.
    3. Otherwise, traverse to the next child node.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 2 |
    | Mastery | ❚❚ |
    | The Latest Attempt | January 15, 2026 |

## Python solution

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        if not root:
            return False

        stack = [(root, root.val)]
        while stack:
            node, val = stack.pop()

            if not node.left and not node.right and val == targetSum:
                return True

            if node.left:
                stack.append((node.left, val + node.left.val))
            if node.right:
                stack.append((node.right, val + node.right.val))

        return False
```

---

[Back to Day 14 Binary Tree IV](index.md) · [All Algorithm notes](../index.md)
