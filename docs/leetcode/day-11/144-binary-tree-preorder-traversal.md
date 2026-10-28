---
hide:
  - toc
---

# 144. Binary Tree Preorder Traversal

*Edited by Charles*

**Easy · Python · Binary Tree**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/binary-tree-preorder-traversal/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Note (middle, left, right):

    The very typical traversal way in the binary tree. For a tree [6, 5, 2, 4, 3, null, null, null, null, 9, null],

## Python solution

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []

        stack = []
        result = []
        cur = root

        while cur or stack:
            if cur:
                stack.append(root.val)
                cur = cur.left
            else:
                cur = stack.pop()
                result.append(node.val)
                cur = cur.right

        return result
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note (middle, left, right):

    1. The very typical traversal way in the binary tree.
    2. For a tree [6, 5, 2, 4, 3, null, null, null, null, 9, null],
        1. push the root node to the stack [6
        2. iterate the stack as long as it is not empty. Retrieve the node at the top of the stack. Save the value to the result list. Push the right node to the stack [2, and then push the left node to the stack [2, 4.
        3. In this way, the value to be saved in the result list is in the preorder traversal.

    ```python
    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, val=0, left=None, right=None):
    #         self.val = val
    #         self.left = left
    #         self.right = right

    class Solution:
        def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
            if not root:
                return []

            stack = [root]
            result = []

            while stack:
                node = stack.pop()
                result.append(node.val)

                if node.right:
                    stack.append(node.right)
                if node.left:
                    stack.append(node.left)

            return result
    ```

    The postorder traversal (middle, right, left) just need to swap two lines of the left node and the right node, and finally reverse the result list.

    ```python
    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, val=0, left=None, right=None):
    #         self.val = val
    #         self.left = left
    #         self.right = right

    class Solution:
        def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
            if not root:
                return []

            stack = [root]
            result = []

            while stack:
                node = stack.pop()
                result.append(node.val)

                if node.left:
                    stack.append(node.left)
                if node.right:
                    stack.append(node.right)

            return result[::-1]
    ```

    The inorder traversal (left, middle, right) iterate all nodes on the left first and then right nodes from deeper layers to shallow layers.

    1. For a tree [6, 5, 2, 4, 3, null, null, null, null, 9, null], initiate the stack and result with empty arrays, but initiate a pointer pointing to the root node.
    2. Push the pointer node to the stack, [6. Let the pointer point to the next left node, 5.
    3. Push the pointer node to the stack, [6, 5. Let the pointer point to the next left node, 4.
    4. Push the pointer node to the stack, [6, 5, 4. Let the pointer point to the next left node, None.
    5. Since the pointer now is null, pop the top node in the stack out, add its value to the result list [4] and let the pointer point to the next right node, None.
    6. Since the pointer now is null, pop the top node in the stack out, add its value to the result list [4, 5] and let the pointer point to the next right node, 3.
    7. Push the pointer node to the stack, [6, 3. Let the pointer point to the next left node, None.
    8. Since the pointer now is null, pop the top node in the stack out, add its value to the result list [4, 5, 3] and let the pointer point to the next right node, None.
    9. ……

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 12, 2026 |

---

[Back to Day 11 Binary Tree I](index.md) · [All Algorithm notes](../index.md)
