# Quick Sort

## One-sentence definition
Quick Sort is a divide‑and‑conquer sorting algorithm that picks a pivot element, partitions the array so elements smaller than the pivot go left and larger go right, then recursively sorts the two subarrays.

## When to use it (use cases)
- When average‑case performance matters more than worst‑case (it’s often faster than merge sort in practice due to lower constant factors).
- When you need an **in‑place** sort with no extra memory (unlike merge sort).
- When working with arrays (cache‑friendly) – but not great for linked lists.
- When you can randomize pivot choice to avoid worst‑case O(n²).
- In system libraries (e.g., C’s `qsort`, Java’s `Arrays.sort` for primitives).

## Step-by-step explanation (plain language, no code first)

1. **Choose a pivot** – pick an element from the array (commonly the last, first, or a random element).

2. **Partition** – rearrange the array so that:
   - All elements **less than** the pivot come before it.
   - All elements **greater than** the pivot come after it.
   - The pivot ends up in its correct sorted position.

3. **Recursively apply** – repeat the process on the left subarray (elements before pivot) and the right subarray (elements after pivot).

4. **Base case** – when a subarray has 0 or 1 element, it’s already sorted.

> **Analogy:** You have a stack of exam papers. Pick one as a “benchmark” (pivot). Throw all papers with lower marks to the left, higher marks to the right. Then repeat on each pile.

## Templates (Python)

### Quick Sort with last element as pivot (simple but not optimal)
```python
def quick_sort(arr, low, high):
    if low < high:
        pi = partition(arr, low, high)
        quick_sort(arr, low, pi - 1)
        quick_sort(arr, pi + 1, high)

def partition(arr, low, high):
    pivot = arr[high]
    i = low - 1
    for j in range(low, high):
        if arr[j] <= pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    arr[i + 1], arr[high] = arr[high], arr[i + 1]
    return i + 1
```

### Quick Sort with random pivot (avoids worst‑case on sorted arrays)
```python
import random

def quick_sort_random(arr, low, high):
    if low < high:
        # randomly choose pivot and swap with last
        rand_idx = random.randint(low, high)
        arr[rand_idx], arr[high] = arr[high], arr[rand_idx]
        pi = partition(arr, low, high)
        quick_sort_random(arr, low, pi - 1)
        quick_sort_random(arr, pi + 1, high)
```

### Helper partition function (same as above)
```python
def partition(arr, low, high):
    pivot = arr[high]
    i = low - 1
    for j in range(low, high):
        if arr[j] <= pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    arr[i + 1], arr[high] = arr[high], arr[i + 1]
    return i + 1
```

## Time & space complexity (Big O)
- **Time (average & best):** `O(n log n)`  
- **Time (worst):** `O(n²)` – when pivot is always the smallest or largest (e.g., already sorted array with last‑element pivot).  
- **Space:** `O(log n)` average recursion depth (in‑place, no extra array).  
- **Randomized pivot** makes worst‑case extremely unlikely.

## Practice questions (concept check)
1. Why is Quick Sort faster than Merge Sort in practice despite both being O(n log n)?
2. What causes the worst‑case O(n²) performance, and how can you avoid it?
3. Is Quick Sort stable? Why or why not?

<details>
<summary>Answers</summary>

1. Quick Sort has better cache locality (works in‑place) and lower constant factors. Merge Sort often allocates extra arrays and copies more.

2. The worst case occurs when the pivot is always the smallest or largest element, causing unbalanced partitions (e.g., already sorted array). You avoid it by using a random pivot or median‑of‑three pivot selection.

3. No, Quick Sort is not stable because the partition step swaps elements from far apart, potentially changing the relative order of equal elements.

</details>

## Practice questions (LeetCode/Codeforces)
1. [Sort an Array](https://leetcode.com/problems/sort-an-array/) – implement Quick Sort to pass.
2. [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/) – uses Quick Select (partition logic).
3. [Sort Colors](https://leetcode.com/problems/sort-colors/) – Dutch national flag problem (three‑way partition).
4. [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/) – can use Quick Select.

## One thing that was confusing to me
I struggled with the partition logic – why `i` starts at `low - 1` and why we swap at the end.  
Eventually I understood: `i` marks the boundary of elements smaller than the pivot. As we scan with `j`, whenever we find an element ≤ pivot, we extend the “smaller” region by swapping into position `i+1`. At the end, the pivot (at `high`) is swapped into the position after the last smaller element. Drawing the array step‑by‑step on paper made it click.

## See also
- [Merge Sort](../sorting/merge-sort.md) – the other major divide‑and‑conquer sort.
- [Heap Sort](../sorting/heap-sort.md) – another O(n log n) in‑place sort (unstable).
- [Three‑way Partitioning (Dutch National Flag)](../sorting/dutch-flag.md) – useful for duplicates.
