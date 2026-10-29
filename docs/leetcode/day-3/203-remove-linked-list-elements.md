---
hide:
  - toc
---

# 203. Remove Linked List Elements

*Edited by Charles*

**Easy · Python · LinkedList**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/remove-linked-list-elements/description/).

    The official page contains the problem statement, examples, and constraints.

??? note "Explanation, examples & other approaches"

    !!! tip "Core idea"

        Define an output linked list, dummy, with the head’s value 0 by default and the next linked node as the head of the given linked list. Define a pointer, cur, pointing to the head of dummy.

    Original study notes follow, including intermediate snippets and earlier attempts.

    1. Define an output linked list, dummy, with the head’s value 0 by default and the next linked node as the head of the given linked list.
    2. Define a pointer, cur, pointing to the head of dummy.
    3. Start a while loop at cur.next. If its value is val, jump to cur.next.next. If not, link the cursor’s next to the current cursor.
    4. Since the head of the output linked list is still 0, use the head’s next as the head of the final output.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 4 |
    | Mastery | ❚❚❚❚ |
    | The Latest Attempt | January 3, 2026 |

## Python solution

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        dummy = ListNode(next = head)

        cur = dummy

        while cur.next:
            if cur.next.val == val:
                cur.next = cur.next.next
            else:
            # We need this else.
            # If there is no else, and implement
            # cur = cur.next for each iteration
            # it is very possible that cur jumps to a None
            # and then the while loop condition will cast
            # an error since None does not have next
            # e.g. [1, 2, 6, 3, 4, 5, 6]
                cur = cur.next

        return dummy.next
```

---

[Back to Day 3 Linked List I](index.md) · [All Algorithm notes](../index.md)
