---
hide:
  - toc
---

# 232. Implement Queue Using Stacks

*Edited by Charles*

**Easy · Python · Stack**

[Original problem](https://leetcode.com/problems/implement-queue-using-stacks/description/)

!!! tip "Core idea"

    Queue: first in first out; Stack: first in last out. Create two arrays, inStack and outStack respectively for pushing and popping.

## Python solution

```python
class MyQueue:

    def __init__(self):
        self.inStack = []
        self.outStack = []

    def push(self, x: int) -> None:
        self.inStack.append(x)

    def pop(self) -> int:
        self.peek()
        return self.outStack.pop()

    def peek(self) -> int:
        if not self.outStack:
            while self.inStack:
                self.outStack.append(self.inStack.pop())
        return self.outStack[-1]

    def empty(self) -> bool:
        return not self.inStack and not self.outStack


# Your MyQueue object will be instantiated and called as such:
# obj = MyQueue()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.peek()
# param_4 = obj.empty()
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. Queue: first in first out; Stack: first in last out.
    2. Create two arrays, inStack and outStack respectively for pushing and popping.
    3. For pop(), we need to ensure outStack is not empty. Thus, we use peek() to reversely stuff the elements in inStack to outStack. Thus, we simply call outStack.pop().
    4. For peek(), we need to ensure outStack is not empty, since the last element is always the front of the queue. As long as outStack is empty, we reverse and move all elements so far in inStack to outStack.
    5. For empty(), we need to both check inStack and outStack are empty.

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 9, 2026 |
    | Feedback | Strongly Recommended for Multiple Attempts |

---

[Back to Day 9 Stack & Queue I](index.md) · [All LeetCode notes](../index.md)
