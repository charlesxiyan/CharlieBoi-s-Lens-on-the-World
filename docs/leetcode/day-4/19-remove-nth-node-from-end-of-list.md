---
hide:
  - toc
---

# 19. Remove Nth Node From End of List

*Edited by Charles*

**Medium · Python · LinkedList**

[Original problem](https://leetcode.com/problems/remove-nth-node-from-end-of-list/description/)

!!! tip "Core idea"

    In order to know the Nth node from the end, we have to know the size of the linked list. Define a virtual head to connect the true head, since the question requires to return the whole adjusted list. Define a pointer current, with its next as the true head.

## Python solution

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        dummy = ListNode(next = head)
        size = 0

        current = dummy
        while current.next:
            current = current.next
            size += 1

        current = dummy
        for _ in range(size - n):
            current = current.next

        current.next = current.next.next

        return dummy.next
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    1. In order to know the Nth node from the end, we have to know the size of the linked list.
    2. Define a virtual head to connect the true head, since the question requires to return the whole adjusted list. Define a pointer current, with its next as the true head.
    3. Iterate the whole list to count the size.
    4. Reset the pointer current to the virtual head.
    5. Iterate the list for size - n times (Python is 0-indexed) to find the node exactly in front of the target node.
    6. Redirect the [current.next](http://current.next) to current.next.next.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 2 |
    | Mastery | ❚❚ |
    | The Latest Attempt | January 5, 2026 |

---

[Back to Day 4 Linked List II](index.md) · [All LeetCode notes](../index.md)
