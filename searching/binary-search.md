# Binary Search

## One-sentence definition
Binary Search finds a target value in a **sorted** array by repeatedly dividing the search interval in half, eliminating half of the remaining elements each time.

## When to use it (use cases)
- The data is sorted (or you can afford to sort it once before many queries).
- You need faster than linear search (`O(n)`) – Binary Search runs in `O(log n)`.
- You are searching for a condition that changes from “false” to “true” at a certain point (e.g., first defect, first index where `arr[i] >= target`).
- The answer lies within a known range (integers) and you can check feasibility – **search over output space** (e.g., Koko Eating Bananas, capacity to ship packages).

## Step-by-step explanation (plain language, no code first)
1. Start with two pointers: `left` at the beginning of the sorted list, `right` at the end.
2. While `left` is less than or equal to `right` (or `left < right` depending on what you need):
   - Compute the middle index: `mid = left + (right - left) // 2` (avoids overflow).
   - Compare the value at `mid` with your target.
   - If `arr[mid] == target` → found, return `mid`.
   - If `arr[mid] < target` → target lies to the right, so move `left = mid + 1`.
   - If `arr[mid] > target` → target lies to the left, so move `right = mid - 1`.
3. If the loop ends without finding the target, return a sentinel (e.g., `-1`) or the insertion point.

> **Think of it like looking up a word in a dictionary** – you don’t check every page; you open the middle, decide whether to go left or right, and repeat.

## Templates (Python)

### Standard binary search (find exact value, return index)
```python
def binary_search(nums: list[int], target: int) -> int:
    left, right = 0, len(nums) - 1

    while left <= right:
        mid = left + (right - left) // 2
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            left = mid + 1
        else:
            right = mid - 1

    return -1
```

### Find first position where condition becomes true (lower bound)
```python
def lower_bound(nums: list[int], target: int) -> int:
    """Return first index i such that nums[i] >= target, or len(nums) if all < target."""
    left, right = 0, len(nums)
    while left < right:
        mid = left + (right - left) // 2
        if nums[mid] < target:
            left = mid + 1
        else:
            right = mid
    return left
```

### General template for searching over an output space (e.g., feasibility)
```python
def binary_search_on_answer(low: int, high: int, feasible: callable) -> int:
    """Return smallest value in [low, high] for which feasible(x) is True."""
    while low < high:
        mid = low + (high - low) // 2
        if feasible(mid):
            high = mid
        else:
            low = mid + 1
    return low
```

### Using Python's `bisect` module
```python
import bisect

arr = [1, 3, 3, 5, 6, 8, 17]
idx = bisect.bisect_left(arr, 3)   # first index with arr[i] >= 3 → 1
idx2 = bisect.bisect_right(arr, 3)  # first index with arr[i] > 3 → 3
```

## Time & space complexity (Big O)
- **Time:** `O(log n)` – each step halves the search space.
- **Space:** `O(1)` (iterative version) or `O(log n)` recursion stack (not shown, but avoid recursion for large n).

## Practice questions (concept check)
1. Why must the array be sorted for Binary Search to work?
2. What happens if you use `left < right` instead of `left <= right` in the standard version?
3. In the general feasibility template, what does `feasible(mid)` represent?

<details>
<summary>Answers</summary>

1. Binary Search relies on ordering to decide whether to go left or right. Without sorting, the comparison tells you nothing about where the target might be.
2. `left < right` will never examine the last remaining element when `left == right`, potentially missing the target if it is at that last index.
3. `feasible(mid)` is a function that returns `True` if `mid` satisfies some condition (e.g., enough capacity, first defect). We search for the first `mid` where it becomes `True`.

</details>

## Practice questions (LeetCode/Codeforces)
1. [Binary Search](https://leetcode.com/problems/binary-search/) – classic exact value search.
2. [Search Insert Position](https://leetcode.com/problems/search-insert-position/) – use lower bound.
3. [Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/) – combine `bisect_left` and `bisect_right`.
4. [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/) – search over output space (eating speed).
5. [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/) – binary search with a twist.
6. [H-Index II](https://leetcode.com/problems/h-index-ii/) – search on a sorted citations array.

## One thing that was confusing to me
For a long time I was confused about when to use `left <= right` versus `left < right`.  
I learned that:
- Use `left <= right` when you want to search for an **exact match** and you need to examine the element when `left == right`.
- Use `left < right` when you are trying to **narrow down to a single candidate** (like the lower bound or feasibility search) and you always keep the answer inside `[left, right]`.  
The loop invariant is key – practicing each variant on simple examples (e.g., array of length 1, target not present) helped me finally get it.

## See also
- [Linear Search](../searching/linear-search.md) – the naive alternative.
- [Ternary Search](../searching/ternary-search.md) – for unimodal functions (less common).
- [bisect module documentation](https://docs.python.org/3/library/bisect.html)
```
