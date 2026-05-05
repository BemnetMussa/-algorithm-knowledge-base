# Singly Linked List

## One-sentence definition
A singly linked list is a chain of nodes where each node stores a value and a single pointer to the next node.

## When to use it (use cases)
- You only need forward traversal and simple inserts at the head.
- Classic interview patterns: reverse, merge sorted lists, cycle detection, middle node.
- Building a stack by pushing/popping at the head (`O(1)`).

## Step-by-step explanation
1. Each node has `val` and `next` (points to the next node or `None`).
2. `head` refers to the first node; walk `cur = cur.next` until `None`.
3. Insert after a known node by rewiring `next` pointers (no shifting like arrays).
4. Delete by making `prev.next` skip the removed node.
5. Use two pointers (fast/slow) for middle and cycle problems.

## Visual intuition (ASCII)

### List shape
```text
head
  |
  v
(1) -> (2) -> (3) -> None
```

### Insert at head
```text
Before:
head -> (2) -> (3) -> None

After:
head -> (1) -> (2) -> (3) -> None
```

### Delete by skipping
```text
Before:
prev -> (x) -> nxt -> ...

After:
prev ---------> nxt -> ...
```

### Reverse one step (save `nxt` first)
```text
prev   cur -> nxt -> ...
        |
        v
prev <- cur    nxt -> ...
```

### Fast / slow (middle)
```text
(1) -> (2) -> (3) -> (4) -> None
 ^s
 ^f
...
        ^s
              ^f
```

### Cycle (Floyd)
```text
(1) -> (2) -> (3) -> (4)
              ^           |
              |___________|
```

## Templates (Python)
### 1) Node + traversal + insert at head
Use when: basic construction and traversal template.
Input: head node and values.
Output: updated head / Python list view.
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def insert_head(head, val):
    # O(1): new node becomes head.
    return ListNode(val, head)

def to_list(head):
    out = []
    cur = head
    while cur:
        out.append(cur.val)
        cur = cur.next
    return out
```

### 2) Reverse (iterative)
Use when: reverse a list in-place in one pass.
Input: head node.
Output: new head of reversed list.
```python
def reverse_list(head):
    prev = None
    cur = head

    while cur:
        nxt = cur.next      # Save next pointer first.
        cur.next = prev     # Reverse current link.
        prev = cur
        cur = nxt

    return prev
```

### 3) Middle node (fast / slow)
Use when: find middle in one pass.
Input: head node.
Output: middle node (second middle for even length).
```python
def middle_node(head):
    # Returns second middle when length is even.
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    return slow
```

### 4) Cycle detection (Floyd)
Use when: check if linked list contains a cycle.
Input: head node.
Output: boolean.
```python
def has_cycle(head):
    # Floyd: if cycle exists, slow/fast eventually meet.
    slow = fast = head

    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            return True

    return False
```

### 5) Merge two sorted lists
Use when: merge two sorted linked lists into one sorted list.
Input: two sorted list heads.
Output: head of merged sorted list.
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

    # At most one list remains; attach it directly.
    tail.next = l1 if l1 else l2
    return dummy.next
```

## Time & space complexity (Big O)
- Access by index: `O(n)`.
- Insert/delete at head: `O(1)`.
- Traversal / search: `O(n)`.
- Reverse: `O(n)` time, `O(1)` extra space.

## Practice questions (concept check)
1. Why is random access `O(n)` for linked lists but `O(1)` for arrays?
2. Why does a dummy (sentinel) head simplify merge and many delete problems?
3. What does fast/slow achieve that one pointer cannot?

<details>
<summary>Answers</summary>

1. You must walk pointers from `head`; there is no indexable memory block.
2. You always have a non-null node before the real list, so fewer `head` special cases.
3. Relative speeds detect cycles and approximate the middle in one pass.

</details>

## Practice questions (LeetCode)
1. [Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group) *(Hard)*
2. [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists) *(Hard)*
3. [LFU Cache](https://leetcode.com/problems/lfu-cache) *(Hard; often list + auxiliary structures)*
4. [Sort List](https://leetcode.com/problems/sort-list) *(merge sort on linked list)*
5. [Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer) *(pointer-heavy)*

## One thing that was confusing to me
I used to lose nodes when reversing because I set `cur.next` before saving `cur.next` in a temp variable; `nxt = cur.next` first fixes that.

## See also
- [Doubly linked list](./doubly-linked-list.md)
- [Common patterns](./linked-list-patterns.md)
- [Linked list overview](./linked-list.md)
