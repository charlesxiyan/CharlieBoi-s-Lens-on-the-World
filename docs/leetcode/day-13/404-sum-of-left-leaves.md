---
hide:
  - toc
---

# 404. Sum of Left Leaves

*Edited by Charles*

**Easy · Python · Binary Tree**

[Original problem](https://leetcode.com/problems/sum-of-left-leaves/)

!!! tip "Core idea"

    Preorder Traverse. Reference to [144. Binary Tree Preorder Traversal](../day-11/144-binary-tree-preorder-traversal.md)

## Python solution

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
		def sumOfLeftLeaves(self, root: Optional[TreeNode]) -> int:
				if not root:
						return 0

				leftVal = 0
				leftSum = self.sumOfLeftLeaves(root.left)
				rightSum = self.sumOfLeftLeaves(root.right)

				if root.left and not root.left.left and not root.left.right:
						leftVal = root.left.val

				return leftVal + leftSum + rightSum
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Preorder Traverse. Reference to [144. Binary Tree Preorder Traversal](../day-11/144-binary-tree-preorder-traversal.md)

    Iteration:

    ```python
    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, val=0, left=None, right=None):
    #         self.val = val
    #         self.left = left
    #         self.right = right
    from collections import deque

    class Solution:
        def sumOfLeftLeaves(self, root: Optional[TreeNode]) -> int:
            if not root.left and not root.right:
                return 0

            dq = deque([root])
            result = []

            while dq:
                n = len(dq)

                for _ in range(n):
                    node = dq.popleft()

                    if node.left:
                        dq.append(node.left)
                        if not node.left.left and not node.left.right:
                            result.append(node.left.val)
                    if node.right:
                        dq.append(node.right)

            return sum(result)
    ```

    Recursion

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 13, 2026 |

---

[Back to Day 13 Binary Tree III](index.md) · [All Algorithm notes](../index.md)
