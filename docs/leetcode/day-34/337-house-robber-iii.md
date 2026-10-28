---
hide:
  - toc
---

# 337. House Robber III

*Edited by Charles*

**Medium · Python · Dynamic Programming**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/house-robber-iii/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    - This is a **Tree DP** problem.
    - For each node, there are only **two states**:
    - Rob this node.
    - Do not rob this node.
    - Compute these two states for every node using **postorder traversal**.

## Python solution

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def rob(self, root: Optional[TreeNode]) -> int:
        res = self.traverse(root)
        return max(res)

    def traverse(self, node):
        if not node:
            return [0, 0] # [偷,不偷]

        left = self.traverse(node.left)
        right = self.traverse(node.right)

        # 偷当前节点, 不偷子节点
        robVal = node.val + left[1] + right[1]
        # 不偷当前节点, 偷子节点
        noVal = max(left) + max(right)

        return [robVal, noVal]
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    ## Idea

    - This is a **Tree DP** problem.
    - For each node, there are only **two states**:
        - Rob this node.
        - Do not rob this node.
    - Compute these two states for every node using **postorder traversal**.

    ## DP Definition

    For each node, return:

    [rob, notRob]

    - rob = maximum money if the current node is robbed.
    - notRob = maximum money if the current node is not robbed.

    ## Base Case

    If the node is `None`:

    return [0, 0]

    ## Transition

    ### Rob current node

    If the current node is robbed, its children cannot be robbed.

    rob = node.val + left[1] + right[1]

    ### Do not rob current node

    If the current node is not robbed, each child can be either robbed or skipped.

    notRob = max(left) + max(right)

    ## Why postorder traversal?

    The DP state of the current node depends on the DP states of its children.

    Therefore, compute the left and right subtrees first, then compute the current node.

    (Postorder: Left → Right → Node)

    ## Final Answer

    After processing the root:

    return max(root.rob, root.notRob)

    ## Complexity

    Time: O(n)

    Space: O(h)

    - n = number of nodes
    - h = height of the tree (recursion stack)

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |

---

[Back to Day 34 Dynamic Programming VII](index.md) · [All Algorithm notes](../index.md)
