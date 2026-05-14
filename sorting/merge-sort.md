# Merge Sort

## One-sentence definition
Merge Sort is a divide‑and‑conquer sorting algorithm that splits an array into halves, recursively sorts each half, then merges the two sorted halves back together.

## When to use it (use cases)
- When you need a **stable sort** (relative order of equal elements is preserved).
- When you’re working with **linked lists** (merge sort works well on linked lists without extra memory).
- When you need **guaranteed O(n log n)** performance (unlike quicksort which can degrade to O(n²) on bad pivots).
- When you want to teach or understand the divide‑and‑conquer paradigm.
- When external sorting is needed (e.g., sorting large files that don’t fit in memory).

## Step-by-step explanation (plain language, no code first)

1. **Divide** – If the array has 0 or 1 element, it’s already sorted (base case). Otherwise, find the middle index and split the array into two halves: left and right.

2. **Conquer (recursive)** – Recursively apply merge sort to the left half and to the right half. This continues until each half has a single element.

3. **Merge** – Take the two sorted halves and combine them into one sorted array:
   - Compare the first elements of both halves.
   - Take the smaller one and place it into a new temporary array.
   - Move forward in that half and repeat until one half is exhausted.
   - Append all remaining elements from the other half.

4. **Copy back** – Copy the merged result back into the original array (or return it if using a functional style).

> **Analogy:** You have two sorted piles of cards. You repeatedly look at the top of both piles, pick the smaller card, and place it face down in a new pile. When one pile is empty, you dump the other pile. That new pile is the merged, sorted set.

## Templates (Python)

### Classic merge sort (in‑place merging into a temporary array)
```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    
    return merge(left, right)

def merge(left, right):
    result = []
    i = j = 0
    
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    
    # Append remaining elements
    result.extend(left[i:])
    result.extend(right[j:])
    return result
```

### In‑place merge sort (saves memory, uses helper array)
```python
def merge_sort_inplace(arr, left=0, right=None):
    if right is None:
        right = len(arr) - 1
    if left < right:
        mid = (left + right) // 2
        merge_sort_inplace(arr, left, mid)
        merge_sort_inplace(arr, mid + 1, right)
        merge_inplace(arr, left, mid, right)

def merge_inplace(arr, left, mid, right):
    # Create temporary copy of the two halves
    left_part = arr[left:mid + 1]
    right_part = arr[mid + 1:right + 1]
    
    i = j = 0
    k = left
    
    while i < len(left_part) and j < len(right_part):
        if left_part[i] <= right_part[j]:
            arr[k] = left_part[i]
            i += 1
        else:
            arr[k] = right_part[j]
            j += 1
        k += 1
    
    while i < len(left_part):
        arr[k] = left_part[i]
        i += 1
        k += 1
    while j < len(right_part):
        arr[k] = right_part[j]
        j += 1
        k += 1
```

## Time & space complexity (Big O)
- **Time:** `O(n log n)` – best, average, and worst case (always splits evenly).
- **Space:** `O(n)` – because of the temporary array used for merging (or `O(log n)` recursion stack in the in‑place version with careful implementation, but merging still needs `O(n)` auxiliary memory in typical implementations).

## Practice questions (concept check)
1. Why does merge sort guarantee `O(n log n)` even in the worst case?
2. What is the extra space requirement, and why is it needed?
3. Is merge sort stable? Why or why not?

<details>
<summary>Answers</summary>

1. The array is always split into two equal halves (log n levels). At each level, merging all elements costs O(n). So total = O(n log n). No bad pivot choice can ruin it, unlike quicksort.

2. O(n) extra space. Merging two sorted halves into a new sorted array requires temporarily storing at least one of the halves (or a whole copy) because you’re writing into a new space to avoid overwriting elements before they are used.

3. Yes, merge sort is stable if implemented correctly. In the `merge` function, when `left[i] <= right[j]`, we take from the left half first. This preserves the original order of equal elements.

</details>

## Practice questions (LeetCode/Codeforces)
1. [Sort an Array](https://leetcode.com/problems/sort-an-array/) – implement merge sort to pass.
2. [Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/) – practice the merge step.
3. [Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/) – a hard problem solvable with merge sort (count inversions variant).
4. [Inversion Count](https://www.geeksforgeeks.org/inversion-count-using-merge-sort/) – standard problem using merge sort.

## One thing that was confusing to me
At first, I didn’t understand why merge sort needs extra memory. I thought, “Why can’t we just merge in place without a temporary array?”  
Trying to do in‑place merging without extra space is very complex (e.g., block‑swap merges) and usually not O(n log n) or stable. The simplest, clean implementation uses O(n) extra space, which is acceptable for most problems. The trade‑off is that we get guaranteed speed and simplicity.

## See also
- [Quick Sort](../sorting/quick-sort.md) – another divide‑and‑conquer sorting algorithm (unstable, in‑place, with average O(n log n) but worst O(n²)).
- [Recursion](../recursion/recursion.md) – merge sort is a perfect recursive example.
``
