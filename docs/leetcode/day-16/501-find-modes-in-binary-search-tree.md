---
hide:
  - toc
---

# 501. Find Modes in Binary Search Tree

*Edited by Charles*

**Easy · Python · Binary Tree**

[Original problem](https://leetcode.com/problems/find-mode-in-binary-search-tree/)

!!! tip "Core idea"

    Inorder traversal visits BST values in sorted order. Count consecutive equal values and retain every value matching the highest frequency.

## Python solution

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def __init__(self):
        self.modes = []
        self.maxFreq = 0
        self.count = 0
        self.prev = None

    def findMode(self, root: Optional[TreeNode]) -> List[int]:
        self.traverse(root)
        return self.modes

    def traverse(self, node):
        if not node:
            return

        self.traverse(node.left)

        if not self.prev:
            self.count = 1
        elif node.val == self.prev.val:
            self.count += 1
        else:
            self.count = 1

        if self.count > self.maxFreq:
            self.modes = [node.val]
            self.maxFreq = self.count
        elif self.count == self.maxFreq:
            self.modes.append(node.val)

        self.prev = node

        self.traverse(node.right)
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Iteration:

    Based on the core idea of the inorder traversal, we need to deal with such cases (the inorder traversal guarantees the list of traversal node values is non-descending. Thus we only need to compare pre and cur to count the modes.):

    1. pre is None, that means cur is still going to find the most left node.
    2. cur.val = pre.val, that means in the inorder traversal we found duplicates.
    3. cur.val ≠ pre.val, that means there won’t be any identical values as pre.val.

    For the count, we use maxFreq to record the highest frequency so far.

    ```python
    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, val=0, left=None, right=None):
    #         self.val = val
    #         self.left = left
    #         self.right = right
    class Solution:
        def findMode(self, root: Optional[TreeNode]) -> List[int]:
            modes = []
            stack = []
            cur = root
            pre = None
            count = 0
            maxFreq = 0

            while cur or stack:
                if cur:
                    stack.append(cur)
                    cur = cur.left
                else:
                    cur = stack.pop()
                    if not pre:
                        count = 1
                    elif cur.val == pre.val:
                        count += 1
                    else:
                        count = 1

                    if count == maxFreq:
                        modes.append(cur.val)
                    if count > maxFreq:
                        modes = [cur.val]
                        maxFreq = count

                    pre = cur
                    cur = cur.right

            return modes
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 16, 2026 |

---

[Back to Day 16 Binary Tree VI](index.md) · [All LeetCode notes](../index.md)
