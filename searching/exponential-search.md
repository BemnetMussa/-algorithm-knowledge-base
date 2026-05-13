
# Exponential Search

## One-sentence definition
Exponential Search finds a target in a sorted array by first finding a range where the target must lie (doubling the range size each step), then applying Binary Search inside that range.

## When to use it (use cases)
- The array is sorted but you don't know the length (e.g., infinite stream, huge array where length is expensive to compute).
- The target is likely to be near the beginning – exponential search finds it faster than binary search in that case.
- You already have a working binary search and want to adapt it to unbounded or very large inputs.
- Real‑world: searching in linked sorted lists with unknown size, log‑structured merge‑trees (databases).

## Step-by-step explanation (plain language, no code first)
1. **Check the first element** – if it equals the target, return immediately.
2. **Find the range** – start with a range of size 1 (index 0 to 1).  
   While the element at the right end of the range is less than the target, **double the range** (move right to `right * 2`).  
   This grows exponentially: 1, 2, 4, 8, 16, … until the right end is beyond the target or the array ends.
3. **Now you have a range** `[left, right]` where the target must be (if it exists).  
   `left` is the previous range’s right boundary (or half of current `right`).  
   `right` is the smallest index where `arr[right] >= target` (or the array length if we hit the end).
4. **Apply binary search** inside `[left, right]` to find the exact target.

> **Analogy:** Looking for a person in a long, sorted line. Instead of measuring the whole line, you first walk 1 step, then 2, then 4, then 8, … until you overshoot them. Then you binary search between your last two positions.

## Templates (Python)

### Exponential Search with built‑in binary search
```python
def exponential_search(arr, target):
    if not arr:
        return -1
    
    # Step 1: handle first element
    if arr[0] == target:
        return 0
    
    # Step 2: find the range
    n = len(arr)
    right = 1
    while right < n and arr[right] < target:
        right *= 2   # double the range
    
    # Step 3: set binary search boundaries
    left = right // 2
    right = min(right, n - 1)
    
    # Step 4: binary search inside [left, right]
    while left <= right:
        mid = left + (right - left) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

### Exponential Search for unknown length (stream / infinite array)
```python
def exponential_search_infinite(get_element, target):
    """
    get_element(i) returns the value at index i, or raises an exception when out of bounds.
    """
    if get_element(0) == target:
        return 0
    
    right = 1
    while True:
        try:
            val = get_element(right)
            if val == target:
                return right
            if val < target:
                right *= 2
            else:
                break
        except (IndexError, StopIteration):
            break
    
    # Binary search between right//2 and right
    left = right // 2
    while left <= right:
        mid = left + (right - left) // 2
        try:
            val = get_element(mid)
            if val == target:
                return mid
            elif val < target:
                left = mid + 1
            else:
                right = mid - 1
        except (IndexError, StopIteration):
            right = mid - 1
    return -1
```

## Time & space complexity (Big O)
- **Time:** `O(log i)` where `i` is the index of the target.  
  - Range‑finding step: `O(log i)` because we double until we reach index ≥ i.  
  - Binary search step: `O(log i)` inside a range of size at most `2i`.  
  - Total: `O(log i)` – faster than binary search when the target is very early (`i` small), same asymptotically otherwise.
- **Space:** `O(1)` (iterative, no recursion).

## Practice questions (concept check)
1. Why does exponential search first check the element at index 0 before the loop?
2. What happens if the target is larger than every element in the array?
3. How does exponential search behave differently from binary search when the array is small (e.g., length 5)?

<details>
<summary>Answers</summary>

1. Because the doubling loop starts from index 1; checking index 0 avoids a duplicate check and handles the case where the target is at the very beginning.
2. The `while right < n and arr[right] < target` will stop when `right` reaches the last index (or exceeds it). Then binary search will run on the full remainder of the array and eventually return `-1`.
3. For tiny arrays, exponential search may do unnecessary work (like doubling past the array bounds) while binary search directly goes to `log n` steps. Exponential search shines when the array is huge or its length is unknown.

</details>

## Practice questions (LeetCode/Codeforces)
1. [Search in a Sorted Array of Unknown Size](https://leetcode.com/problems/search-in-a-sorted-array-of-unknown-size/) – classic exponential search problem.
2. [Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/) – can use exponential to find range, then binary search.
3. [Peak Index in a Mountain Array](https://leetcode.com/problems/peak-index-in-a-mountain-array/) – not directly exponential, but shows the idea of narrowing search space.

## One thing that was confusing to me
At first, I didn't understand why we start with `right = 1` and then double. Why not start with a larger initial range?  
The beauty is: if the target is at index 0, we catch it immediately. If it’s at index 1 or 2, we find it in very few steps. The worst‑case number of steps (even for huge arrays) is still `O(log i)`. Doubling from 1 is optimal because any constant multiplier would be slower for small indices. This “start small, explode” pattern is what makes it exponential.

## See also
- [Binary Search](../searching/binary-search.md) – the second stage of exponential search.
