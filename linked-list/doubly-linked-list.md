# Doubly Linked List

## One-sentence definition
A doubly linked list connects each node to both its successor and predecessor, so you can walk forward and backward and often delete a known node in `O(1)`.

## When to use it (use cases)
- You need backward traversal or removal when you already hold a reference to the node.
- LRU / LFU caches: move a node to the “most recent” position in `O(1)` with a hash map.
- Browser-style history, editors with cursor structures, or any “detach and reattach” workflow.

## Step-by-step explanation
1. Each node stores `val`, `prev`, and `next`.
2. Maintain `head` (and often `tail`) or use sentinel nodes to avoid edge cases.
3. On insert, wire four links: new node’s `prev`/`next`, and neighbors’ pointers back.
4. On delete of node `x`, connect `x.prev.next = x.next` and `x.next.prev = x.prev` (with null checks if no sentinel).
5. Always update `head` / `tail` when inserting or removing at the ends.

## Visual intuition (ASCII)

### Shape (with `None` at both ends)
```text
None <-> (1) <-> (2) <-> (3) <-> None
         ^head
```

### Insert after node `a` (new node `n`)
```text
... <-> (a) <-> (b) <-> ...
         \       /
           (n)
becomes:
... <-> (a) <-> (n) <-> (b) <-> ...
```

### Remove node `x` in the middle
```text
... <-> (a) <-> (x) <-> (b) <-> ...
becomes:
... <-> (a) <-> (b) <-> ...
```

## Templates (Python)
### 1) Node + insert at head + remove node (given reference)
Use when: direct pointer rewiring with optional null checks.
Input: head/node references and value.
Output: updated head or detached node.
```python
class DListNode:
    def __init__(self, val=0, prev=None, next=None):
        self.val = val
        self.prev = prev
        self.next = next

def insert_head(head, val):
    # O(1) head insert.
    node = DListNode(val, None, head)
    if head:
        head.prev = node
    return node  # new head

def unlink_node(node):
    # O(1) detach when node reference is known.
    if node.prev:
        node.prev.next = node.next
    if node.next:
        node.next.prev = node.prev
    node.prev = node.next = None
```

### 2) Sentinel head + tail (dummy nodes, fewer null checks)
Use when: you want cleaner code with minimal edge-case branching.
Input: operations on a maintained doubly list object.
Output: updated list state and node handles.
```python
class DoublyLinkedList:
    def __init__(self):
        self.head = DListNode(0)  # sentinel
        self.tail = DListNode(0)  # sentinel
        self.head.next = self.tail
        self.tail.prev = self.head

    def append_last(self, val):
        # Insert before tail sentinel (always valid position).
        node = DListNode(val, self.tail.prev, self.tail)
        self.tail.prev.next = node
        self.tail.prev = node
        return node  # return handle for O(1) LRU moves

    def move_to_front(self, node):
        # LRU-style move: detach then place right after head.
        self._unlink(node)
        nxt = self.head.next
        self.head.next = node
        node.prev = self.head
        node.next = nxt
        nxt.prev = node

    def _unlink(self, node):
        # Internal helper: assumes node is not a sentinel.
        node.prev.next = node.next
        node.next.prev = node.prev
```

### 3) LRU-style sketch (doubly list + map): get / put
Use when: constant-time cache lookup + recency updates are required.
```python
# Pattern: map[key] -> DListNode with (key, value).
# List order: head (MRU) ... tail (LRU). Evict from tail side.
# On get: move node to MRU. On put new: add MRU; if over capacity, pop LRU.
```

## Time & space complexity (Big O)
- Insert/delete at a **known node**: `O(1)` pointer updates.
- Search by value without a map: `O(n)`.
- Extra space: `O(n)` nodes; often `+O(n)` hash map for key → node (LRU).

## Practice questions (concept check)
1. Why can doubly linked lists delete a node in `O(1)` when singly lists usually need `O(n)`?
2. What breaks if you forget to update **both** `prev` and `next` when inserting?
3. Why do LRU caches pair a doubly linked list with a hash map?

<details>
<summary>Answers</summary>

1. You can reach the predecessor directly via `node.prev`; singly lists need a scan for `prev`.
2. You get broken links or “dangling” pointers; the list may form cycles or lose nodes.
3. The map finds the node for a key in `O(1)`; the list maintains eviction order in `O(1)` moves.

</details>

## Practice questions (LeetCode)
1. [LRU Cache](https://leetcode.com/problems/lru-cache)
2. [Design Browser History](https://leetcode.com/problems/design-browser-history)
3. [Flatten a Multilevel Doubly Linked List](https://leetcode.com/problems/flatten-a-multilevel-doubly-linked-list)

## One thing that was confusing to me
Sentinel nodes felt like extra memory at first, but they remove almost all `if head is None` branches when inserting and deleting.

## See also
- [Singly linked list](./singly-linked-list.md)
- [Common patterns](./linked-list-patterns.md)
- [Linked list overview](./linked-list.md)
