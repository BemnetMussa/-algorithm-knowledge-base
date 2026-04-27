# Depth-First Search (DFS)

## One-sentence definition
DFS explores a graph by going as deep as possible along each branch before backtracking.

## When to use it (use cases)
- Traverse all nodes in a graph or tree.
- Find connected components.
- Detect cycles in directed or undirected graphs.
- Solve grid problems (islands, regions) using flood-fill style traversal.
- Run backtracking/search-style problems.

## Step-by-step explanation
1. Start from a source node and mark it as visited.
2. Move to one unvisited neighbor.
3. Repeat this process (go deeper) until no more unvisited neighbors exist.
4. Backtrack to the previous node and continue with other unvisited neighbors.
5. Stop when all reachable nodes are visited.

## Templates (Python)
### 1) Recursive DFS (graph traversal)
```python
def dfs_recursive(graph, node, visited, order):
    visited.add(node)
    order.append(node)

    for neighbor in graph.get(node, []):
        if neighbor not in visited:
            dfs_recursive(graph, neighbor, visited, order)
```

### 2) Iterative DFS (stack)
```python
def dfs_iterative(graph, start):
    visited = set()
    stack = [start]
    order = []

    while stack:
        node = stack.pop()
        if node in visited:
            continue

        visited.add(node)
        order.append(node)

        for neighbor in reversed(graph.get(node, [])):
            if neighbor not in visited:
                stack.append(neighbor)

    return order
```

### 3) DFS coloring (white/gray/black)
```python
def has_cycle_directed(graph):
    color = {}  # 0=unvisited, 1=visiting, 2=done

    def dfs(node):
        color[node] = 1
        for nei in graph.get(node, []):
            if color.get(nei, 0) == 1:
                return True
            if color.get(nei, 0) == 0 and dfs(nei):
                return True
        color[node] = 2
        return False

    for node in graph:
        if color.get(node, 0) == 0 and dfs(node):
            return True
    return False
```

### 4) DFS cycle detection (undirected graph)
```python
def has_cycle_undirected(graph):
    visited = set()

    def dfs(node, parent):
        visited.add(node)
        for nei in graph.get(node, []):
            if nei not in visited:
                if dfs(nei, node):
                    return True
            elif nei != parent:
                return True
        return False

    for node in graph:
        if node not in visited and dfs(node, -1):
            return True
    return False
```

## Time & space complexity (Big O)
- Time: `O(V + E)` where `V` is number of vertices and `E` is number of edges.
- Space: `O(V)` for visited + recursion stack (or explicit stack).

## Practice questions (concept check)
1. What is the key difference in traversal behavior between DFS and BFS?
2. Why can recursive DFS hit recursion depth limits on large inputs?
3. In undirected graphs, why do we pass the `parent` node during cycle detection?
4. What does the gray (`visiting`) color represent in DFS coloring?

<details>
<summary>Answers</summary>

1. DFS goes deep first, while BFS explores level by level.
2. Deep recursion may exceed call stack limits, causing runtime errors.
3. To avoid treating the immediate back edge to parent as a cycle.
4. The node is currently in the recursion path; seeing it again means a back edge/cycle.

</details>

## Practice questions (LeetCode)
1. [Max Area of Island](https://leetcode.com/problems/max-area-of-island)
2. [Surrounded Regions](https://leetcode.com/problems/surrounded-regions)
3. [Number of Provinces](https://leetcode.com/problems/number-of-provinces)
4. [Sum Root to Leaf Numbers](https://leetcode.com/problems/sum-root-to-leaf-numbers)
5. [Evaluate Division](https://leetcode.com/problems/evaluate-division)

## One thing that was confusing to me
At first, I was confused about when to choose recursive DFS vs iterative DFS; iterative DFS helps avoid recursion-depth issues on very deep graphs.

## See also
- [Breadth-First Search (BFS)](./bfs.md)
- [Topological Sort](./topological-sort.md)
