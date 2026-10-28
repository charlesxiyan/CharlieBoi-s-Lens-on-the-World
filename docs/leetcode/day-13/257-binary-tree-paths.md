---
hide:
  - toc
---

# 257. Binary Tree Paths

*Edited by Charles*

**Easy · Python · Binary Tree**

[Original problem](https://leetcode.com/problems/binary-tree-paths/)

!!! tip "Core idea"

    Recursion

    stack and pathStack are updated simultaneously.

## Python solution

```python
# 递归

# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:
        self.res = []
        def c(n,path=''):
            if n and n.left is None and n.right is None:
                cur = path + ' ' + str(n.val)
                self.res.append(cur)
                return
            if n:
                cur = path + ' ' + str(n.val)
                c(n.left,cur)
                c(n.right,cur)

        c(root)
        for i in range(len(self.res)):
            self.res[i] = "->".join(self.res[i].split())
        return self.res
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Recursion

    stack and pathStack are updated simultaneously.

    ```python
    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, val=0, left=None, right=None):
    #         self.val = val
    #         self.left = left
    #         self.right = right
    from collections import deque

    class Solution:
        def __init__(self):
            self.path = []
            self.result = []

        def binaryTreePaths(self, root: Optional[TreeNode])-> List[str]:
            self.traverse(root)
            return self.result

        def traverse(self, node):
            if not node:
                return

            if not node.left and not node.right:
                self.path.append(str(node.val))
                self.result.append("->".join(self.path))
                self.path.pop()

            self.path.append(str(node.val))

            self.traverse(node.left)
            self.traverse(node.right)

            self.path.pop()
    ```

    Iteration

    stack and pathStack are updated simultaneously.

    ```python
    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, val=0, left=None, right=None):
    #         self.val = val
    #         self.left = left
    #         self.right = right
    class Solution:
    		def binaryTreePaths(self, root: Optional[TreeNode])-> List[str]:
    				stack = [root]
    				pathStack = [str(root.val)]
    				result = []

    				while stack:
    						node = stack.pop()
    						path = pathStack.pop()

    						if not node.left and not node.right:
    								result.append(path)

    						if node.left:
    								stack.append(node.left)
    								pathStack.append(path + '->' str(node.left.val))
    						if node.right:
    								stack.append(node.right)
    								pathStack.append(path + '->' str(node.right.val))

    				return result
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 13, 2026 |
    | Feedback | This question requires multiple reviews! The logic look similar but the detail of dealing with paths needs more attention. |

---

[Back to Day 13 Binary Tree III](index.md) · [All Algorithm notes](../index.md)
