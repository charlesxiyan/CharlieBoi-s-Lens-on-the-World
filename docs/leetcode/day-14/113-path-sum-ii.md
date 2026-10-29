---
hide:
  - toc
---

# 113. Path Sum II

*Edited by Charles*

**Medium · Python · Binary Tree**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/path-sum-ii/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        Recursion:

        Attention: in self.result.append(list(path)), it has to be list(path), since this creates a copy and saves the copy in self.result. No matter what later operation to path, the copy would not be influenced; otherwise, if we use self.result.append(path), path.pop() will also change the list saved in self.result, even though “append” operation is done before path.pop().

    Original study notes follow, including intermediate snippets and earlier attempts.

    Recursion:

    1. Attention: in self.result.append(list(path)), it has to be list(path), since this creates a copy and saves the copy in self.result. No matter what later operation to path, the copy would not be influenced; otherwise, if we use self.result.append(path), path.pop() will also change the list saved in self.result, even though “append” operation is done before path.pop().

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
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
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> List[List[int]]:
        self.result = []
        self.traverse(root, targetSum, [])

        return self.result

    def traverse(self, node, remaining, path):
        if not node:
            return
        path.append(node.val)
        remaining -= node.val

        if not node.left and not node.right and remaining == 0:
            self.result.append(list(path))

        self.traverse(node.left, remaining, path)
        self.traverse(node.right, remaining, path)

        path.pop()
```

---

[Back to Day 14 Binary Tree IV](index.md) · [All Algorithm notes](../index.md)
