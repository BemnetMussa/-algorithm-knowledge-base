# Heap

## One-sentence definition
A heap is a complete binary tree (usually implemented with an array) that keeps the smallest or largest element at the root for fast access.

## When to use it (use cases)
- Repeatedly get/remove minimum or maximum element.
- Top-`k` problems (largest/smallest/frequent).
- Priority scheduling (tasks by priority/time).
- Streaming data (running median or dynamic extremes).
- Efficient graph algorithms (Dijkstra, Prim) with priority queues.

## Step-by-step explanation
1. Choose heap type:
   - **Min-heap**: parent <= children (root is smallest).
   - **Max-heap**: parent >= children (root is largest).
2. Insert by appending to the array, then bubble up (`heapify-up`).
3. Remove top by swapping root with last element, popping last, then bubble down (`heapify-down`).
4. Build heap from list in linear time using bottom-up heapify.
5. For top-`k`, keep a heap of size `k` and evict when needed.

## Visual intuition (ASCII)

### Min-heap property
```text
        2
      /   \
     5     8
    / \   /
   9  12 14

Every parent <= its children.
```

### Array representation
```text
Heap array: [2, 5, 8, 9, 12, 14]
index i:
left child  = 2*i + 1
right child = 2*i + 2
parent      = (i - 1) // 2
```

## Templates (Python)
### 1) Min-heap basics with `heapq`
```python
import heapq

def min_heap_demo(nums):
    heap = nums[:]
    heapq.heapify(heap)  # O(n) build

    heapq.heappush(heap, 4)     # O(log n)
    smallest = heapq.heappop(heap)  # O(log n)
    return smallest, heap
```

### 2) Max-heap pattern in Python (negate values)
```python
import heapq

def max_heap_demo(nums):
    heap = [-x for x in nums]
    heapq.heapify(heap)

    heapq.heappush(heap, -10)
    largest = -heapq.heappop(heap)
    return largest
```

### 3) Top-k largest elements (min-heap of size `k`)
```python
import heapq

def top_k_largest(nums, k):
    heap = []

    for x in nums:
        if len(heap) < k:
            heapq.heappush(heap, x)
        elif x > heap[0]:
            # Replace smallest among top-k.
            heapq.heapreplace(heap, x)

    return sorted(heap, reverse=True)
```

### 4) Kth largest element
```python
import heapq

def kth_largest(nums, k):
    heap = nums[:k]
    heapq.heapify(heap)

    for x in nums[k:]:
        if x > heap[0]:
            heapq.heapreplace(heap, x)

    return heap[0]
```

### 5) Merge k sorted lists (heap by current value)
```python
import heapq

def merge_k_sorted_lists(lists):
    heap = []
    result = []

    for i, arr in enumerate(lists):
        if arr:
            # (value, list_index, element_index)
            heapq.heappush(heap, (arr[0], i, 0))

    while heap:
        val, li, ei = heapq.heappop(heap)
        result.append(val)

        ni = ei + 1
        if ni < len(lists[li]):
            heapq.heappush(heap, (lists[li][ni], li, ni))

    return result
```

## Time & space complexity (Big O)
- Peek top: `O(1)`
- Insert (`push`): `O(log n)`
- Remove top (`pop`): `O(log n)`
- Build heap (`heapify`): `O(n)`
- Top-k with size `k` heap over `n` values: `O(n log k)`

## Practice questions (concept check)
1. Why is `heapify` `O(n)` and not `O(n log n)`?
2. Why does a min-heap of size `k` help in top-k largest problems?
3. Why does heap sort use `O(1)` extra space (array version) but Python `heapq` workflows may use extra memory?

<details>
<summary>Answers</summary>

1. Most nodes are near leaves and need little/no sinking, so total work sums to linear.
2. The smallest among current top-k stays at root for fast comparison/replacement.
3. In-place heap sort rearranges one array; practical `heapq` patterns often keep additional heaps or copies.

</details>

## Practice questions (LeetCode)
1. [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array)
2. [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements)
3. [Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream)
4. [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists)
5. [IPO](https://leetcode.com/problems/ipo)

## One thing that was confusing to me
At first, I expected heaps to return fully sorted output directly, but I learned a heap guarantees order only at the root (global min/max), not between all siblings/subtrees.

## See also
- [Prefix Sum](../prefix-sum/prefix-sum.md)
- [Topological Sort](../graph/topological-sort.md)
