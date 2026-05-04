# Linked List (overview)

## One-sentence definition
A linked list stores elements in nodes connected by pointers instead of a single contiguous block of memory.

## What lives in this folder
Notes are split by variant and by common patterns so each file stays focused and matches the rest of the knowledge base.

| Note | What it covers |
| --- | --- |
| [Singly linked list](./singly-linked-list.md) | One `next` pointer per node; reversal, merge, fast/slow. |
| [Doubly linked list](./doubly-linked-list.md) | `prev` + `next`; O(1) removal given a node reference. |
| [Common patterns](./linked-list-patterns.md) | Stack/queue built from lists, sentinels, LRU-style (list + map). |

## When to use linked lists (in general)
- Frequent inserts/deletes when you already hold a node reference.
- Pointer-heavy interview problems (reverse segments, cycles, merge).
- Building blocks for stacks, queues, and cache eviction order (often doubly linked).

## See also
- [Sliding Window](../sliding-window/sliding-window.md)
- [Depth-First Search (DFS)](../graph/dfs.md)
