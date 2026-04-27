# Sliding Window

## One-sentence definition
The Sliding Window technique solves subarray/substring problems efficiently by maintaining a moving range of elements instead of recomputing results from scratch for every possible range.

## When to use it (use cases)
- Finding the maximum or minimum sum of a subarray of fixed size `k`.
- Finding the longest substring with at most `k` distinct characters.
- Any problem asking about a contiguous subarray or substring that satisfies some condition.
- When a brute-force nested loop solution exists but is too slow (O(n²) → O(n)).

## Step-by-step explanation (plain language, no code first)

There are two flavors: **fixed-size window** and **variable-size window**.

### Fixed-size window (window size = k)
1. Add the first `k` elements to form your initial window and compute the result.
2. Slide the window one step to the right: add the new element coming in on the right, remove the element going out on the left.
3. Update your result after each slide.
4. Repeat until the right edge of the window reaches the end of the array.

### Variable-size window (expand/shrink)
1. Start with both `left` and `right` pointers at index 0.
2. Expand the window by moving `right` forward and including the new element.
3. If the window violates the condition (e.g., sum exceeds a target), shrink it by moving `left` forward until the condition is satisfied again.
4. Track the best result (longest, shortest, max sum, etc.) after each valid state.
5. Repeat until `right` reaches the end of the array.

The key insight: instead of recomputing the whole window each time, you only update based on what entered and what left.

## Visual (ASCII)

```
Array:  [2, 1, 5, 1, 3, 2],  k = 3

Step 1: [2, 1, 5] 1  3  2   → sum = 8
Step 2:  2 [1, 5, 1] 3  2   → sum = 7  (−2, +1)
Step 3:  2  1 [5, 1, 3] 2   → sum = 9  (−1, +3)
Step 4:  2  1  5 [1, 3, 2]  → sum = 6  (−5, +2)

Maximum sum = 9
```

## Code example (Python)

```python
# --- Fixed-size window: maximum sum subarray of size k ---

def max_sum_subarray(arr: list[int], k: int) -> int:
    n = len(arr)
    if n < k:
        return -1

    # Build the first window
    window_sum = sum(arr[:k])
    max_sum = window_sum

    # Slide the window
    for i in range(k, n):
        window_sum += arr[i] - arr[i - k]   # add right, remove left
        max_sum = max(max_sum, window_sum)

    return max_sum


# --- Variable-size window: longest subarray with sum <= target ---

def longest_subarray_with_sum(arr: list[int], target: int) -> int:
    left = 0
    current_sum = 0
    max_length = 0

    for right in range(len(arr)):
        current_sum += arr[right]            # expand window

        while current_sum > target:          # shrink if violated
            current_sum -= arr[left]
            left += 1

        max_length = max(max_length, right - left + 1)

    return max_length


# Quick tests
print(max_sum_subarray([2, 1, 5, 1, 3, 2], 3))   # → 9
print(longest_subarray_with_sum([1, 2, 3, 4, 5], 9))  # → 3  ([2,3,4] or [1,3,5]... actually [1,2,3,4]=10 no → [2,3,4]=9 ✓)
```

## Time & space complexity (Big O)

| Variant | Time | Space |
|---|---|---|
| Fixed-size window | O(n) | O(1) |
| Variable-size window | O(n) — each element enters and leaves the window at most once | O(1) |

Brute-force comparison: a nested loop checking all subarrays is O(n²) or O(n³). Sliding window gets you the same answer in O(n).

## Practice questions

1. [Maximum Average Subarray I — LeetCode 643](https://leetcode.com/problems/maximum-average-subarray-i/)
2. [Longest Substring Without Repeating Characters — LeetCode 3](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
3. [Minimum Size Subarray Sum — LeetCode 209](https://leetcode.com/problems/minimum-size-subarray-sum/)

<details>
<summary>Answers</summary>

1. Use a fixed-size sliding window. Build the first window of size `k`, then slide right: add `arr[i]`, subtract `arr[i-k]`, track the running maximum. Time: O(n), Space: O(1).

2. Use a variable-size window with a `set` (or frequency map) to track characters. Expand `right`; when a duplicate is found, shrink from `left` until the duplicate is removed. Track the longest valid window. Time: O(n), Space: O(min(n, alphabet size)).

3. Use a variable-size window. Expand `right` to grow the sum; once `current_sum >= S`, record the window length and shrink from `left` to see if a shorter valid window exists. Keep the minimum valid length seen. Time: O(n), Space: O(1).

</details>

## One thing that was confusing to me
The variable-size window's time complexity looks like O(n²) because of the inner `while` loop, but it is actually O(n). Each element is added to the window exactly once (when `right` passes over it) and removed at most once (when `left` passes over it), so the total number of operations across the entire loop is 2n, which is O(n).

## See also
- [Two Pointers](../two-pointers/two-pointers.md)
- [Prefix Sum](../prefix-sum/prefix-sum.md)
