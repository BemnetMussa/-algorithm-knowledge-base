# Breadth-First Search (BFS)

## One-sentence definition
BFS explores a graph level by level from a start node, visiting all nearest nodes first before moving farther away.

## When to use it (use cases)
- Find the shortest path in an unweighted graph.
- Traverse all nodes by distance from a source.
- Check connectivity (whether a node can be reached).
- Perform level-order traversal in trees.

## Step-by-step explanation
1. Start from a source node and mark it as visited.
2. Put the source node into a queue.
3. While the queue is not empty:
   - Remove the front node.
   - Process it (print/store/check target).
   - For each unvisited neighbor, mark visited and add it to the queue.
4. Stop when the queue is empty (or when your target is found).

## Templates (Python)
### 1) Standard BFS (graph traversal)
```python
from collections import deque

def bfs(graph, start):
    visited = set([start])
    order = []
    queue = deque([start])

    while queue:
        node = queue.popleft()
        order.append(node)

        for neighbor in graph.get(node, []):
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)

    return order
```

### 2) Multi-source BFS (all sources start together)
```python
from collections import deque

def multi_source_bfs(graph, sources):
    dist = {}
    queue = deque()

    for s in sources:
        dist[s] = 0
        queue.append(s)

    while queue:
        node = queue.popleft()
        for nei in graph.get(node, []):
            if nei not in dist:
                dist[nei] = dist[node] + 1
                queue.append(nei)

    return dist
```

### 3) Level-order BFS (process nodes by layers)
```python
from collections import deque

def bfs_levels(graph, start):
    visited = {start}
    queue = deque([start])
    levels = []

    while queue:
        level_size = len(queue)
        current_level = []

        for _ in range(level_size):
            node = queue.popleft()
            current_level.append(node)
            for nei in graph.get(node, []):
                if nei not in visited:
                    visited.add(nei)
                    queue.append(nei)

        levels.append(current_level)

    return levels
```

### 4) Bidirectional BFS (shortest path between two nodes)
```python
from collections import deque

def bidirectional_bfs(graph, start, target):
    if start == target:
        return 0

    front = {start}
    back = {target}
    visited = {start, target}
    steps = 0

    while front and back:
        if len(front) > len(back):
            front, back = back, front

        next_front = set()
        for node in front:
            for nei in graph.get(node, []):
                if nei in back:
                    return steps + 1
                if nei not in visited:
                    visited.add(nei)
                    next_front.add(nei)

        front = next_front
        steps += 1

    return -1
```

## Time & space complexity (Big O)
- Time: `O(V + E)` where `V` is number of vertices and `E` is number of edges.
- Space: `O(V)` for the visited set and queue.

## Practice questions (concept check)
1. Why do we use a queue in BFS instead of a stack?
2. In what type of graph does BFS guarantee the shortest path?
3. What can go wrong if we mark nodes as visited only when popping from the queue?

<details>
<summary>Answers</summary>

1. Queue gives FIFO behavior, which processes nodes in level order.
2. In an unweighted graph (or a graph where all edge weights are equal).
3. The same node may be added to the queue multiple times, causing extra work.

</details>

## Practice questions (LeetCode)
1. [Shortest Path in Binary Matrix](https://leetcode.com/problems/shortest-path-in-binary-matrix)
2. [01 Matrix](https://leetcode.com/problems/01-matrix)
3. [Word Ladder](https://leetcode.com/problems/word-ladder)

## One thing that was confusing to me
At first, I mixed up BFS and DFS because both visit all nodes; BFS is the one that expands by levels using a queue.

## See also
- [Depth-First Search (DFS)](./dfs.md)
- [Shortest Path (Unweighted Graph)](./shortest-path-unweighted.md)
