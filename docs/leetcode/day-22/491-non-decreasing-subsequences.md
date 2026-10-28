---
hide:
  - toc
---

# 491. Non-decreasing Subsequences

*Edited by Charles*

**Medium · Python · Backtrack**

[Original problem](https://leetcode.com/problems/non-decreasing-subsequences/)

!!! tip "Core idea"

    Recall [90. Subsets II](../day-21/90-subsets-ii.md) for the core idea. This question does not allow duplicates. Thus, within the same level of recurrence, we need to deal with duplicates here. For example, nums = [4, 7, 7, 1]. The 1st level generates [4], the recurrence makes the program dive into the 2nd level, which generates [4, 7], [4, 7] and [4, 1], and these three subsets will be sent to the 3rd level by the recurrence to generate [4, 7, 7], [4, 7, 1]; [4, 7, 1]. The 4th level will generate [4, 7, 7, 1] by the first subset in the 3rd level. We can see how the duplicates occur. Hence, we prune within each recurrence.

## Python solution

```python
class Solution:
    def findSubsequences(self, nums: List[int]) -> List[List[int]]:
        result = []
        self.backtrack(nums, 0, [], result)
        return result

    def backtrack(self, nums, startIndex, path, result):
        if len(path) >= 2:
            result.append(path[:])

        uset = set()
        for i in range(startIndex, len(nums)):
            if (path and path[-1] > nums[i]) or nums[i] in uset:
                continue
            uset.add(nums[i])
            path.append(nums[i])
            self.backtrack(nums, i + 1, path, result)
            path.pop()
```

??? note "Explanation, examples & other approaches"

    Original study notes follow, including intermediate snippets and earlier attempts.

    Note:

    1. Recall [90. Subsets II](../day-21/90-subsets-ii.md) for the core idea.
    2. This question does not allow duplicates. Thus, within the same level of recurrence, we need to deal with duplicates here. For example, nums = [4, 7, 7, 1]. The 1st level generates [4], the recurrence makes the program dive into the 2nd level, which generates [4, 7], [4, 7] and [4, 1], and these three subsets will be sent to the 3rd level by the recurrence to generate [4, 7, 7], [4, 7, 1]; [4, 7, 1]. The 4th level will generate [4, 7, 7, 1] by the first subset in the 3rd level. We can see how the duplicates occur. Hence, we prune within each recurrence.



    ```python
    [4, 7, 7, 1]

    backtrack(nums, 0, [])
    	if -> False

    	uset = ()
    	for i in range(0, 4):
    			if -> False
    			uset = (4)
    			path = [4]
    			backtrack(nums, 1, [4])
    					if -> False
    					uset = ()
    					for i in range(1, 4):
    							i = 1
    							if -> False
    							uset = (7)
    							path = [4, 7]
    							backtrack(nums, 2, [4, 7])
    									if -> True
    											res = [[4, 7]]
    									uset = ()
    									for i in range(2, 4):
    											i = 2
    											if -> False
    											uset = (7)
    											path = [4, 7, 7]
    											backtrack(nums, 3, [4, 7, 7])
    													if -> True
    															res = [[4, 7], [4, 7, 7]]
    													for i in range(3, 4):
    															if -> True
    																	continue
    											path = [4, 7]

    											i = 3
    											if -> True: path[-1] > nums[i]
    													continue
    							i = 2
    							if ->
    ```

??? note "Study record"

    | Field | Value |
    | --- | --- |
    | Attempts | 1 |
    | Mastery | ❚ |
    | The Latest Attempt | January 24, 2026 |

---

[Back to Day 22 Backtrack IV](index.md) · [All Algorithm notes](../index.md)
