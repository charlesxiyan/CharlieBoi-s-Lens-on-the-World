---
hide:
  - toc
---

# 206. Reverse Linked List

*Edited by Charles*

**Easy · Python · LinkedList**

[Original problem](https://leetcode.com/problems/reverse-linked-list/description/)

!!! tip "Core idea"

    Main logic:

    Define two pointers: previous and current. Current points to the head and previous points to None.

## Python solution

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        current = head
        previous = None
        while current:
            temp = current.next
            current.next = previous
            previous = current
            current = temp

        return previous
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Main logic:

    1. Define two pointers: previous and current.
    2. Current points to the head and previous points to None.
    3. Iterate the whole linked list:
        1. define temp to save current.next, since we need current to move forward after making the current node point to the previous.
        2. let [current.next](http://current.next) be previous (reversion is done)
        3. let previous be current (move both previous and current)
        4. let current be temp

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 3 |
    | Mastery | ❚❚❚ |
    | The Latest Attempt | January 4, 2026 |

---

[Back to Day 3 Linked List I](index.md) · [All LeetCode notes](../index.md)
