# Linked List — Common Patterns

## One-sentence definition
These patterns reuse linked lists as stacks, queues, or as the “order” half of a hash-backed structure.

## When to use each pattern
- **Stack from singly list:** LIFO with push/pop at the head (`O(1)`).
- **Queue from singly list:** FIFO with `head` dequeue and `tail` enqueue (`O(1)` if you keep `tail`).
- **Dummy / sentinel head:** merge, remove duplicates, partition — fewer `head` edge cases.
- **Doubly list + hash map:** LRU / frequency structures; move node to front or bucket in `O(1)`.

## Step-by-step explanation
1. Pick the operations you need (push only vs enqueue+dequeue).
2. If you need both ends, prefer doubly linked or `head`+`tail` singly queue.
3. For “find then update order,” store map: key → node pointer.
4. Keep invariants explicit: after every operation, `tail.next is None`, sentinels closed, etc.

## Visual intuition (ASCII)

### Stack (singly): push/pop at head
```text
push 3:  head -> (3) -> (2) -> None
pop:     head -> (2) -> None
```

### Queue (singly): dequeue head, enqueue at tail
```text
head -> (1) -> (2) -> (3) <- tail
dequeue: head -> (2) -> (3) <- tail
enqueue 4 at tail
```

### Dummy head before real list
```text
dummy -> (1) -> (2) -> None
        ^ always non-null anchor
```

## Templates (Python)
### 1) Stack using singly linked list (head = top)
Use when: you need LIFO with `O(1)` push/pop.
Input: values to push/pop.
Output: popped value or `None`.
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class LinkedStack:
    def __init__(self):
        self.head = None

    def push(self, val):
        # O(1): push at head.
        self.head = ListNode(val, self.head)

    def pop(self):
        if not self.head:
            return None
        val = self.head.val
        self.head = self.head.next
        return val
```

### 2) Queue using singly linked list (`head` + `tail`)
Use when: you need FIFO with `O(1)` enqueue/dequeue.
Input: values to enqueue.
Output: dequeued value or `None`.
```python
class LinkedQueue:
    def __init__(self):
        self.head = self.tail = None

    def enqueue(self, val):
        # O(1): append at tail.
        node = ListNode(val)
        if self.tail:
            self.tail.next = node
            self.tail = node
        else:
            self.head = self.tail = node

    def dequeue(self):
        # O(1): remove from head.
        if not self.head:
            return None
        val = self.head.val
        self.head = self.head.next
        if self.head is None:
            self.tail = None
        return val
```

### 3) LRU-style: doubly list order + `dict` for O(1) lookup
Use when: you need recency-aware cache eviction.
```python
# dict[key] = DListNode(key, value)
# List: MRU near one sentinel, LRU near the other; evict LRU on overflow.
# See doubly-linked-list.md for unlink / insert helpers.
```

## Time & space complexity (Big O)
- Stack push/pop: `O(1)` time, `O(n)` nodes.
- Queue enqueue/dequeue: `O(1)` with `tail` pointer.
- LRU get/put (map + doubly list): `O(1)` average per op.

## Practice questions (concept check)
1. Why is a singly linked “queue” painful without a `tail` pointer?
2. When is a doubly linked list worth the extra `prev` pointer per node?
3. What role does the dummy head play in merge or partition problems?

<details>
<summary>Answers</summary>

1. Enqueue at the end becomes `O(n)` if you must walk from `head` to find the last node.
2. When you must splice out or move an arbitrary node in `O(1)` (LRU, editors, history).
3. It gives a stable predecessor for the first real node, simplifying inserts/removes.

</details>

## Practice questions (LeetCode)
1. [LRU Cache](https://leetcode.com/problems/lru-cache)
2. [Design Browser History](https://leetcode.com/problems/design-browser-history)
3. [Design Linked List](https://leetcode.com/problems/design-linked-list)

## One thing that was confusing to me
I mixed up “queue from two stacks” (LeetCode style) with “queue from head+tail”; both are valid, but the head+tail list is the direct pattern.

## See also
- [Singly linked list](./singly-linked-list.md)
- [Doubly linked list](./doubly-linked-list.md)
- [Linked list overview](./linked-list.md)
