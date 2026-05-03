# Linked List

## One-sentence definition
A Linked List is a linear data structure where each node stores a value and a pointer to the next node (and sometimes previous node).

## When to use it (use cases)
- Frequent insertions/deletions in the middle when node references are known.
- Problems that require pointer manipulation (reverse, merge, cycle detection).
- Building blocks for stacks, queues, and hash collision chains.
- Situations where dynamic growth is needed without contiguous memory layout.

## Step-by-step explanation
1. Each node contains `value` and `next` pointer.
2. `head` points to the first node; traversal follows `next` until `None`.
3. Insert by rewiring pointers (instead of shifting elements like arrays).
4. Delete by skipping a node using `prev.next = prev.next.next`.
5. Use fast/slow pointers for cycle and middle-related patterns.

## Visual intuition (ASCII)

### Singly linked list shape
```text
head
  |
  v
(1) -> (2) -> (3) -> None
```

### Insert at head (new node becomes `head`)
```text
Before:
head -> (2) -> (3) -> None

After insert_head(head, 1):
head -> (1) -> (2) -> (3) -> None
```

### Delete by skipping a node
```text
Before:
prev -> (x) -> nxt -> ...

After:
prev ---------> nxt -> ...
```

### Reverse one step (why `nxt` matters)
```text
prev   cur -> nxt -> ...
        |
        v
prev <- cur    nxt -> ...
```

### Fast / slow pointers (middle intuition)
```text
slow moves 1 step, fast moves 2 steps per "tick".

(1) -> (2) -> (3) -> (4) -> None
 ^s
 ^f

(1) -> (2) -> (3) -> (4) -> None
        ^s
              ^f

When fast hits the end, slow is near the middle.
```

### Cycle (Floyd)
```text
(1) -> (2) -> (3) -> (4)
              ^           |
              |___________|
```

## Templates (Python)
### 1) Basic traversal + insertion at head
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def insert_head(head, val):
    # New node points to old head.
    return ListNode(val, head)

def to_list(head):
    out = []
    cur = head
    while cur:
        out.append(cur.val)
        cur = cur.next
    return out
```

### 2) Reverse linked list (iterative)
```python
def reverse_list(head):
    prev = None
    cur = head

    while cur:
        nxt = cur.next      # Save next before rewiring.
        cur.next = prev     # Reverse pointer.
        prev = cur
        cur = nxt

    return prev
```

### 3) Fast/slow pointers (find middle)
```python
def middle_node(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    return slow
```

### 4) Floyd cycle detection
```python
def has_cycle(head):
    slow = fast = head

    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            return True

    return False
```

### 5) Merge two sorted linked lists
```python
def merge_two_lists(l1, l2):
    dummy = ListNode(0)
    tail = dummy

    while l1 and l2:
        if l1.val <= l2.val:
            tail.next = l1
            l1 = l1.next
        else:
            tail.next = l2
            l2 = l2.next
        tail = tail.next

    # Attach remaining nodes.
    tail.next = l1 if l1 else l2
    return dummy.next
```

## Time & space complexity (Big O)
- Access by index: `O(n)` (no random access).
- Insert/delete at head: `O(1)`.
- Search/traversal: `O(n)`.
- Reverse list: `O(n)` time, `O(1)` extra space.

## Practice questions (concept check)
1. Why is linked-list random access `O(n)` while array access is `O(1)`?
2. Why does using a dummy node simplify many linked-list problems?
3. What is the core idea behind fast/slow pointer techniques?

<details>
<summary>Answers</summary>

1. Nodes are not contiguous, so you must traverse pointers from the head to reach index `i`.
2. It removes head-edge-case branching by giving a stable node before the real list.
3. Move one pointer twice as fast to detect cycles, find middles, or split lists.

</details>

## Practice questions (LeetCode)
1. [Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group) *(Hard)*
2. [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists) *(Hard)*
3. [LFU Cache](https://leetcode.com/problems/lfu-cache) *(Hard)*
4. [Sort List](https://leetcode.com/problems/sort-list) *(Medium but important pattern)*
5. [Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer) *(Medium, pointer-heavy)*

## One thing that was confusing to me
At first, I kept losing nodes while reversing because I rewired `next` before storing it; saving `nxt = cur.next` first fixed that bug.

## See also
- [Sliding Window](../sliding-window/sliding-window.md)
- [Depth-First Search (DFS)](../graph/dfs.md)
