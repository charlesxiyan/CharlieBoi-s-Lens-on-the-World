---
hide:
  - toc
---

# 538. Convert BST to Greater Tree

*Edited by Charles*

**Medium · Python · Binary Tree**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/convert-bst-to-greater-tree/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Iteration:

    This question is quite fabulous!

## Python solution

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def convertBST(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        self.prevVal = 0
        self.traverse(root)
        return root

    def traverse(self, node):
        if not node:
            return

        self.traverse(node.right)

        node.val += self.prevVal
        self.prevVal = node.val

        self.traverse(node.left)
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Iteration:

    This question is quite fabulous!

    The inorder traversal of a tree, for example, is [1, 3, 2, 4, 5, 6, 7]. The result should be [28, 27, 24, 22, 18, 13, 7].

    It can be easily found that if we iterate the nodes from right to left and calculate their new values, it is simply a summation from right to the currently iterated node.

    This traversal is the same as inorder but right - middle - left and we need two pointers to record the currently iterated node and the last node.

    ```python
    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, val=0, left=None, right=None):
    #         self.val = val
    #         self.left = left
    #         self.right = right
    class Solution:
        def convertBST(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
            cur = root
            pre = None
            stack = []

            while cur or stack:
                if cur:
                    stack.append(cur)
                    cur = cur.right
                else:
                    cur = stack.pop()

                    if pre:
                        cur.val += pre.val

                    pre = cur
                    cur = cur.left

            return root
    ```

    Recurrence

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 20, 2026 |

---

[Back to Day 18 Binary Tree VIII](index.md) · [All Algorithm notes](../index.md)
