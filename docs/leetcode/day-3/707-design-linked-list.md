---
hide:
  - toc
---

# 707. Design Linked List

*Edited by Charles*

**Medium · Python · LinkedList**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/design-linked-list/description/).

    The official page contains the problem statement, examples, and constraints.

!!! tip "Core idea"

    Define a class to create a ListNode. For initialization in the class MyLinkedList, create a head for the linked list to be created later as well as the size of the list, i.e. [Head] - [0-indexed node] - [1-indexed node] - ……

## Python solution

```python
class ListNode:
    def __init__(self, val = 0, next = None):
        self.val = val
        self.next = next

class MyLinkedList:

    def __init__(self):
        self.dummy = ListNode()
        self.size = 0

    def get(self, index: int) -> int:
        if index < 0 or index > self.size - 1:
            return -1

        current = self.dummy.next
        for i in range(index):
            current = current.next

        return current.val


    def addAtHead(self, val: int) -> None:
        self.dummy.next = ListNode(val = val, next = self.dummy.next)
        self.size += 1

    def addAtTail(self, val: int) -> None:
        current = self.dummy
        while current.next:
            current = current.next

        current.next = ListNode(val = val)
        self.size += 1

    def addAtIndex(self, index: int, val: int) -> None:
		    # index can be self.size at most, i.e. the tail.
        if index < 0 or index > self.size:
            return

        current = self.dummy

        for i in range(index):
            current = current.next

        current.next = ListNode(val = val, next = current.next)
        self.size += 1

    def deleteAtIndex(self, index: int) -> None:
        if index < 0 or index > self.size - 1:
            return

        current = self.dummy

        for i in range(index):
            current = current.next

        current.next = current.next.next
        self.size -= 1

# Your MyLinkedList object will be instantiated and called as such:
# obj = MyLinkedList()
# param_1 = obj.get(index)
# obj.addAtHead(val)
# obj.addAtTail(val)
# obj.addAtIndex(index,val)
# obj.deleteAtIndex(index)
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    1. Define a class to create a ListNode.
    2. For initialization in the class MyLinkedList, create a head for the linked list to be created later as well as the size of the list, i.e. [Head] - [0-indexed node] - [1-indexed node] - ……
    3. For retrieving the value of a node with a given index, define a current pointer pointing to the head’s next. That is, the current pointer stays at 0-indexed node. Now, move the current to the node with the given index. For example, index = 2. In the for loop (for i in range(index)), i = 0, the current points to 1-indexed node; i = 1, the current points to 2-indexed node.
    4. For adding a node to the list, (1) create a node with the given value, and the next pointing to [current.next](http://current.next) (2) make the head/current point to the newly created node (3) update the size.
    5. For adding a node to the tail, (1) create a node with the given value, and the next pointing to None (2) make the current point to the newly created node (3) update the size.
    6. For inserting a node at a given index, (1) move current until the [current.next](http://current.next) is the node with the target index. (2) create a node with the given value, and next pointing to current.next (3) make the current.next be the newly created node. (4) update the size.
    7. For deleting a node at a given index, (1) move current until the [current.next](http://current.next) is the node with the target index. (2) make the current.next be current.next.next. (3) update the size.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 2 |
    | Mastery | ❚❚ |
    | The Latest Attempt | January 4, 2026 |
    | Feedback | Strongly Recommended for Multiple Attempts! |

---

[Back to Day 3 Linked List I](index.md) · [All Algorithm notes](../index.md)
