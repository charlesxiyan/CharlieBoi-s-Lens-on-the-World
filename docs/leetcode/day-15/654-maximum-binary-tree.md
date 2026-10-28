---
hide:
  - toc
---

# 654. Maximum Binary Tree

*Edited by Charles*

**Medium · Python · Binary Tree**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/maximum-binary-tree/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Recursion:

    See [106. Construct Binary Tree from Inorder and Postorder Traversal](../day-14/106-construct-binary-tree-from-inorder-and-postorder-traversal.md) and [105. Construct Binary Tree from Preorder and Inorder Traversal](../day-14/105-construct-binary-tree-from-preorder-and-inorder-traversal.md) for the core idea of recursion.

## Python solution

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def constructMaximumBinaryTree(self, nums: List[int]) -> Optional[TreeNode]:
        if not nums:
            return None

        rootVal = max(nums)
        idx = nums.index(rootVal)

        root = TreeNode(rootVal)
        root.left = self.constructMaximumBinaryTree(nums[: idx])
        root.right = self.constructMaximumBinaryTree(nums[idx + 1:])

        return root
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Recursion:

    See [106. Construct Binary Tree from Inorder and Postorder Traversal](../day-14/106-construct-binary-tree-from-inorder-and-postorder-traversal.md) and [105. Construct Binary Tree from Preorder and Inorder Traversal](../day-14/105-construct-binary-tree-from-preorder-and-inorder-traversal.md) for the core idea of recursion.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 15, 2026 |

---

[Back to Day 15 Binary Tree V](index.md) · [All Algorithm notes](../index.md)
