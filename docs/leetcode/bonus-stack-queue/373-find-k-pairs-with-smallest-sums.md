---
hide:
  - toc
---

# 373. Find K Pairs with Smallest Sums

*Edited by Charles*

**Medium · Python · Heap · Sorted streams**

??? question "Problem"

    [Read the original problem on LeetCode](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/description/).

    Given two nonempty arrays sorted in nondecreasing order, return `k` pairs with the smallest sums, taking one element from each array. Different index pairs may produce identical values.

??? note "Explanation, examples & other approaches"

    ## Approach and Core Idea

    Fix an index `i` in `nums1`. Pairing it with successive elements of `nums2` produces a sorted stream of sums. Merge these streams using a heap that stores only the next candidate from each active row.

    ## Algorithm Steps

    1. Insert `(nums1[i] + nums2[0], i, 0)` for the first `min(k, len(nums1))` rows.
    2. Pop the smallest sum and append its value pair.
    3. Advance only that row by pushing column `j + 1` if it exists.
    4. Stop after `k` results or when the heap is empty.

    ## Hand Simulation

    For `nums1 = [1,7,11]`, `nums2 = [2,4,6]`, `k = 3`, initial sums are `3,9,13`. Pop `3` → `[1,2]` and push `5`; pop `5` → `[1,4]` and push `7`; pop `7` → `[1,6]`. The three smallest pairs all come from the first row.

    ## Important Implementation Details

    Every unseen entry in a row is at least as large as that row’s heap candidate. Thus the heap minimum is globally safe to output. Rows beyond the first `k` are unnecessary: the first `k` row heads already provide `k` sums no greater than any omitted row head. Ties can use any valid ordering. The popped `j` may equal the last column; only the **push of a successor** requires `j + 1 < len(nums2)`. No visited set is needed because each index pair has one predecessor in its row.

    ## Time and Space Complexity

    Let $H=\min(k,\lvert nums1\rvert)$ and $R=\min(k,\lvert nums1\rvert\lvert nums2\rvert)$. Repeated heap insertion and extraction take $O((H+R)\log(H+1))$ time. Auxiliary heap space is $O(H)$, plus $O(R)$ output.

    ## Key Takeaways

    Generate a sorted stream’s successor only when its current candidate is consumed.

## Python solution

```python
import heapq
class Solution:
    def kSmallestPairs(self, nums1: list[int], nums2: list[int], k: int) -> list[list[int]]:
        minHeap = []
        res = []
        for i in range(min(k, len(nums1))):
            heapq.heappush(minHeap, (nums1[i] + nums2[0], i, 0))
        while minHeap and len(res) < k:
            _, i, j = heapq.heappop(minHeap)
            res.append([nums1[i], nums2[j]])
            # Initially, each active nums1[i] is paired only with nums2[0].
            # Since nums2 is sorted, the sum at j is no larger than at j + 1.
            # Lazily push the next pair only after the current pair is popped.
            # A successor exists only when j + 1 < len(nums2).
            if j + 1 < len(nums2):
                heapq.heappush(minHeap, (nums1[i] + nums2[j + 1], i, j + 1))
        return res
```

---

[Back to Bonus Stack & Queue](index.md) · [All Algorithm notes](../index.md)
