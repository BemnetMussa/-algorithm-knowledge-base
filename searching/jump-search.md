# Jump Search

## One-sentence definition
Jump Search works on a sorted array by jumping ahead by a fixed number of steps (block size) until you pass the target, then performing a linear search within the last block.

## When to use it (use cases)
- The array is **sorted** but accessing elements is expensive (e.g., disk reads, network calls) – jumping reduces the number of accesses compared to linear search.
- You want a simple algorithm that doesn’t require the complexity of binary search (no mid‑index calculations, no overflow concerns).
- The array size is known and not too large – Jump Search is easy to implement and predictable.
- As a stepping stone to understanding more advanced search structures like skip lists or B+ trees.

## Step-by-step explanation (plain language, no code first)
1. **Determine the jump block size** – optimal size is √n (square root of array length).
2. **Start at the first element** (index 0).  
   Jump forward by `block_size` until you either:
   - Find an element equal to the target (return index), or
   - Find an element greater than the target (stop, the target must be in the previous block), or
   - Reach the end of the array.
3. **Perform a linear search** inside the last block (from the previous jump position up to the current index, but not exceeding the array bounds).
4. If the linear search finds the target, return its index; otherwise, return -1.

> **Analogy:** Looking for a friend in a long, sorted line. Instead of checking every person, you take big steps (say, 10 people at a time). When you overshoot, you go back a step and then check each person one by one until you find them or pass them again.

## Templates (Python)

### Jump Search implementation
```python
import math

def jump_search(arr, target):
    n = len(arr)
    if n == 0:
        return -1
    
    # Step 1: optimal block size
    block_size = int(math.sqrt(n))
    
    # Step 2: jump to find the block
    prev = 0
    while prev < n and arr[min(prev + block_size, n) - 1] < target:
        prev += block_size
    
    # Step 3: linear search inside the block
    for i in range(prev, min(prev + block_size, n)):
        if arr[i] == target:
            return i
    return -1
```

### Jump Search with early exit (if target found during jumps)
```python
def jump_search_early(arr, target):
    n = len(arr)
    if n == 0:
        return -1
    
    block_size = int(math.sqrt(n))
    step = block_size
    
    # Jump forward, but check each jump point
    while step < n and arr[step] < target:
        step += block_size
    
    # Start linear search from the previous jump position
    start = step - block_size
    for i in range(start, min(step, n)):
        if arr[i] == target:
            return i
    return -1
```

## Time & space complexity (Big O)
- **Time:** `O(√n)` – because you make at most `√n` jumps and at most `√n` linear steps inside one block.
  - More precisely: the optimal block size `√n` minimises `(n/block_size) + block_size`, which is `2√n` → `O(√n)`.
- **Space:** `O(1)` – only a few variables used.

## Practice questions (concept check)
1. Why is `√n` the optimal block size for Jump Search?
2. What happens if the target is larger than every element in the array?
3. When would Jump Search be preferred over Binary Search?

<details>
<summary>Answers</summary>

1. The total number of operations = `(n / block_size)` jumps + `block_size` linear steps. Differentiating gives the minimum at `block_size = √n`. This balances the two parts.
2. The `while` loop will keep jumping until `prev` exceeds the array length; then the linear search runs over the last block (which may be smaller) and eventually returns `-1`.
3. Binary search is faster (`O(log n)`) in general. Jump Search is only “better” in specific scenarios where the cost of jumping is much lower than the cost of computing the middle index (e.g., in some memory models) or when you need a very simple implementation with no risk of off‑by‑one errors.

</details>

## Practice questions (LeetCode/Codeforces)
1. There’s no direct “Jump Search” problem on LeetCode, but you can implement it for:
   - [Search in a Sorted Array of Unknown Size](https://leetcode.com/problems/search-in-a-sorted-array-of-unknown-size/) – exponential search is more common, but Jump Search can be adapted.
   - [Valid Perfect Square](https://leetcode.com/problems/valid-perfect-square/) – use Jump Search to find the square root range, then linear scan.
2. [Kth Missing Positive Number](https://leetcode.com/problems/kth-missing-positive-number/) – can be solved with Jump Search after converting to a sorted missing‑count array.
3. [First Bad Version](https://leetcode.com/problems/first-bad-version/) – not directly, but the range‑finding idea is similar.

## One thing that was confusing to me
At first, I didn't understand why we use `√n` instead of a different number.  
If the block size is too small (e.g., 2), you make many jumps – almost like linear search. If it’s too large (e.g., n/2), your linear scan at the end becomes huge. The magic `√n` balances both, giving the smallest worst‑case total steps. This taught me the idea of **optimising a trade‑off**, which appears everywhere in computer science (e.g., memory vs. time, load balancing).

## See also
- [Binary Search](../searching/binary-search.md) – faster but requires mid‑index calculation.
- [Exponential Search](../searching/exponential-search.md) – also uses a two‑phase approach (range finding + binary search).

Ready to tackle **sorting algorithms** (e.g., Quick Sort, Merge Sort) or **data structures** (e.g., Hash Table, Trie) next? Just say the word.
