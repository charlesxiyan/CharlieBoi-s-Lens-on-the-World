---
hide:
  - toc
---

# 225. Implement Stack Using Queues

*Edited by Charles*

**Easy · Python · Stack**

[Original problem](https://leetcode.com/problems/implement-stack-using-queues/description/)

!!! tip "Core idea"

    Assume the input is [1, 2, 3] in order, and 3 now is the top of the stack. For pop(), we need to move 3 to the left and 1, 2 reinput to the queue from right.

## Python solution

```python
# from collections import deque
class MyStack:
    from collections import deque
    # q1 = [ ]
    # q2 = [ ]
    def __init__(self):
        self.q1 = deque()
        self.q2 = deque()

    def push(self, x: int) -> None:
        self.q2.append(x)
        while self.q1:
            self.q2.append(self.q1.popleft())
        self.q1,self.q2 = self.q2, self.q1

    def pop(self) -> int:
        return self.q1.popleft()
        # self.q1.popleft()
    # push(4), push(8)
    # q1 = [4]
    # q2 = []
    # q1 = [8, 4]
    def top(self) -> int:
        return self.q1[0]

    def empty(self) -> bool:
        return not self.q1 and not self.q2

# Your MyStack object will be instantiated and called as such:
# obj = MyStack()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.top()
# param_4 = obj.empty()
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. Assume the input is [1, 2, 3] in order, and 3 now is the top of the stack.
    2. For pop(), we need to move 3 to the left and 1, 2 reinput to the queue from right.
    3. For top(), we need to initialize a top_element in the beginning of the class to record the top element in the stack, so that we can retrieve top anytime.

    ```python
    from collections import deque

    class MyStack:

        def __init__(self):
            self.queue = deque()
            self.top_element = 0

        def push(self, x: int) -> None:
            self.queue.append(x)
            self.top_element = x

        def pop(self) -> int:
            size = len(self.queue)

            while size > 1:
                self.queue.append(self.queue.popleft())
                size -= 1

            self.top_element = self.queue[-1]

            return self.queue.popleft()

        def top(self) -> int:
            return self.top_element

        def empty(self) -> bool:
            return not self.queue

    # Your MyStack object will be instantiated and called as such:
    # obj = MyStack()
    # obj.push(x)
    # param_2 = obj.pop()
    # param_3 = obj.top()
    # param_4 = obj.empty()
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 9, 2026 |
    | Feedback | Strongly Recommended for Multiple Attempts |

---

[Back to Day 9 Stack & Queue I](index.md) · [All Algorithm notes](../index.md)
