# Prefix Sum

## One-sentence definition
Prefix Sum precomputes cumulative sums so range-sum queries can be answered in constant time.

## When to use it (use cases)
- Fast range sum queries on arrays.
- Subarray sum counting problems.
- 2D matrix region sum queries.
- Problems where transforming repeated sum work into preprocessing is helpful.

## Step-by-step explanation
1. Build a prefix structure where each position stores sum from start up to that position.
2. To get range sum, subtract two prefix values instead of summing the range directly.
3. For 2D, use inclusion-exclusion to combine four prefix cells.
4. For counting subarrays, track prefix sums in a hash map.

## Templates (Python)
### 1) 1D Prefix Sum (range sum query)
```python
def build_prefix(nums):
    prefix = [0] * (len(nums) + 1)
    for i, x in enumerate(nums):
        prefix[i + 1] = prefix[i] + x
    return prefix

def range_sum(prefix, left, right):
    return prefix[right + 1] - prefix[left]
```

### 2) 2D Prefix Sum (matrix region sum)
```python
def build_prefix_2d(matrix):
    if not matrix or not matrix[0]:
        return [[0]]

    rows, cols = len(matrix), len(matrix[0])
    pref = [[0] * (cols + 1) for _ in range(rows + 1)]

    for r in range(rows):
        for c in range(cols):
            pref[r + 1][c + 1] = (
                matrix[r][c]
                + pref[r][c + 1]
                + pref[r + 1][c]
                - pref[r][c]
            )
    return pref

def sum_region(pref, r1, c1, r2, c2):
    return (
        pref[r2 + 1][c2 + 1]
        - pref[r1][c2 + 1]
        - pref[r2 + 1][c1]
        + pref[r1][c1]
    )
```

### 3) Prefix Sum + Hash Map (count subarrays with target sum)
```python
def subarray_sum_equals_k(nums, k):
    count = 0
    prefix = 0
    freq = {0: 1}

    for x in nums:
        prefix += x
        count += freq.get(prefix - k, 0)
        freq[prefix] = freq.get(prefix, 0) + 1

    return count
```

### 4) Difference Array (range updates, then rebuild values)
```python
def apply_range_updates(n, updates):
    diff = [0] * (n + 1)
    for left, right, val in updates:
        diff[left] += val
        if right + 1 < len(diff):
            diff[right + 1] -= val

    arr = [0] * n
    running = 0
    for i in range(n):
        running += diff[i]
        arr[i] = running
    return arr
```

## Time & space complexity (Big O)
- 1D build: `O(n)`, query: `O(1)`, space: `O(n)`.
- 2D build: `O(rows * cols)`, query: `O(1)`, space: `O(rows * cols)`.
- Prefix + hash map counting: `O(n)` time, `O(n)` space.

## Practice questions (concept check)
1. Why is the prefix array usually created with size `n + 1` instead of `n`?
2. For 2D prefix sums, why do we subtract one overlap area and then add it back once?
3. In `subarray sum equals k`, why do we initialize `freq` with `{0: 1}`?

<details>
<summary>Answers</summary>

1. It makes range formulas cleaner and avoids left-bound edge cases.
2. Because inclusion-exclusion prevents double counting the shared rectangle.
3. It counts subarrays starting at index `0` when current prefix itself equals `k`.

</details>

## Practice questions (LeetCode)
1. [Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k)
2. [Range Sum Query 2D - Immutable](https://leetcode.com/problems/range-sum-query-2d-immutable)
3. [Subarrays with K Different Integers](https://leetcode.com/problems/subarrays-with-k-different-integers)

## One thing that was confusing to me
At first, I mixed up when to use normal prefix sums vs prefix-plus-hash-map; I learned hash-map frequency is needed when counting subarrays, not just querying sums.

## See also
- [Sliding Window](../sliding-window/sliding-window.md)
- [Topological Sort](../graph/topological-sort.md)
