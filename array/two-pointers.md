
# Two Pointers Technique

## One-sentence definition
Two Pointers is a technique that uses two pointers (indices) moving through a data structure (usually an array or string) in a single pass to solve problems that would otherwise require nested loops.

## When to use it (use cases)
- Searching for a pair (or triplet) that satisfies a condition in a sorted array (e.g., two sum, three sum).
- Removing duplicates in‑place.
- Reversing an array or string.
- Checking for palindromes.
- Merging two sorted arrays.
- Sliding window problems (often combined with two pointers).
- Partitioning arrays (e.g., move zeros to end, segregate odd/even).

## Step-by-step explanation (plain language, no code first)

There are several common patterns:

### Pattern 1: Opposing pointers (one at start, one at end)
1. Place one pointer at the beginning (left), another at the end (right).
2. While left < right:
   - Perform some check or operation using arr[left] and arr[right].
   - Move left forward or right backward (or both) based on the condition.
3. This is great for sorted arrays or palindrome checking.

### Pattern 2: Fast and slow pointers (both start at the beginning)
1. Place both pointers at the start.
2. One pointer moves faster (e.g., 2 steps at a time) while the other moves slower.
3. Used for cycle detection (Floyd’s cycle detection) or finding middle of linked list.

### Pattern 3: Two pointers scanning from left (both start at beginning, one moves conditionally)
1. One pointer (i) iterates normally.
2. The other pointer (j) only moves when a condition is met.
3. Used for removing duplicates in‑place or moving elements.

> **Analogy:** Looking for two people in a line whose heights sum to a target. One person starts at the left, one at the right. If their heights sum is too small, the left person steps right. If too large, the right person steps left. They meet in the middle.

## Templates (Python)

### Opposing pointers – pair sum in sorted array (find if exists)
```python
def has_pair_sum(nums, target):
    left, right = 0, len(nums) - 1
    while left < right:
        current_sum = nums[left] + nums[right]
        if current_sum == target:
            return True
        elif current_sum < target:
            left += 1
        else:
            right -= 1
    return False
```

### Opposing pointers – reverse array in‑place
```python
def reverse_array(arr):
    left, right = 0, len(arr) - 1
    while left < right:
        arr[left], arr[right] = arr[right], arr[left]
        left += 1
        right -= 1
```

### Fast & slow pointers – find middle of linked list
```python
def find_middle(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    return slow
```

### Pointers scanning left – remove duplicates in‑place (sorted array)
```python
def remove_duplicates(nums):
    if not nums:
        return 0
    j = 0  # points to last unique element
    for i in range(1, len(nums)):
        if nums[i] != nums[j]:
            j += 1
            nums[j] = nums[i]
    return j + 1  # new length
```

## Time & space complexity (Big O)
- **Time:** `O(n)` – each pointer moves at most `n` steps total.
- **Space:** `O(1)` – no extra data structures (modifies input or uses constant variables).

## Practice questions (concept check)
1. Why does the opposing pointers technique require the array to be sorted for pair sum?
2. What happens if you use opposing pointers on an unsorted array?
3. When would you choose fast & slow pointers over opposing pointers?

<details>
<summary>Answers</summary>

1. The decision to move left or right depends on the current sum being too small or too large. That only works if the array is sorted – otherwise you can’t guarantee that moving left increases the sum.

2. The algorithm would give incorrect results because the ordering property is lost. For unsorted arrays, you’d need a hash set or sort first.

3. Fast & slow pointers are used for linked list problems (cycle detection, middle element) or problems where you need two different speeds. Opposing pointers are for arrays where you can index directly.

</details>

## Practice questions (LeetCode/Codeforces)
1. [Two Sum II – Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/) – classic opposing pointers.
2. [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/) – opposing pointers on string.
3. [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/) – scanning left pointers.
4. [Container With Most Water](https://leetcode.com/problems/container-with-most-water/) – opposing pointers.
5. [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/) – fast & slow pointers.
6. [Move Zeroes](https://leetcode.com/problems/move-zeroes/) – scanning left pointers.

## One thing that was confusing to me
I used to think two pointers was just one pattern. But there are multiple: opposing, fast/slow, and same‑direction. Each is for different problem structures. For example, I once tried to use fast/slow on a sorted array pair sum – that didn’t work. Learning which pattern fits which problem type was the real aha moment.

## See also
- [Sliding Window](../sliding-window/sliding-window.md) – often uses two pointers with a window.
- [Binary Search](../searching/binary-search.md) – also uses left/right pointers but different logic.
