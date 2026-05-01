# Topological Sort

## One-sentence definition
Topological Sort gives a linear ordering of nodes in a Directed Acyclic Graph (DAG) such that for every directed edge `u -> v`, `u` appears before `v`.

## When to use it (use cases)
- Order tasks with dependency constraints.
- Check whether all courses/jobs can be finished.
- Build dependency-aware execution order in systems.
- Detect cycles in directed dependency graphs (if ordering is impossible).

## Step-by-step explanation
1. Model the problem as a directed graph where edges mean "must come before."
2. Use either indegree-based BFS (Kahn's algorithm) or DFS postorder.
3. If all nodes are included in the final order, the graph is acyclic.
4. If some nodes are missing, there is a cycle, so topological order does not exist.

## Templates (Python)
### 1) Kahn's Algorithm (BFS + indegree)
```python
from collections import deque

def topological_sort_kahn(n, edges):
    graph = [[] for _ in range(n)]
    indegree = [0] * n

    for u, v in edges:
        graph[u].append(v)
        indegree[v] += 1

    # Start with nodes that currently have no prerequisites.
    queue = deque(i for i in range(n) if indegree[i] == 0)
    order = []

    while queue:
        node = queue.popleft()
        order.append(node)

        for nei in graph[node]:
            indegree[nei] -= 1
            if indegree[nei] == 0:
                queue.append(nei)

    # If not all nodes are processed, a cycle exists.
    return order if len(order) == n else []
```

### 2) Modified DFS (postorder + cycle detection)
```python
def topological_sort_dfs(n, edges):
    graph = [[] for _ in range(n)]
    for u, v in edges:
        graph[u].append(v)

    color = [0] * n  # 0=unvisited, 1=visiting, 2=done
    order = []
    has_cycle = False

    def dfs(node):
        nonlocal has_cycle
        color[node] = 1

        for nei in graph[node]:
            if color[nei] == 0:
                dfs(nei)
                if has_cycle:
                    return
            # Back-edge to "visiting" node indicates a cycle.
            elif color[nei] == 1:
                has_cycle = True
                return

        color[node] = 2
        # Postorder push; reverse at the end for topo order.
        order.append(node)

    for i in range(n):
        if color[i] == 0:
            dfs(i)
            if has_cycle:
                return []

    return order[::-1]
```

## Time & space complexity (Big O)
- Time: `O(V + E)` for both Kahn's and DFS-based approaches.
- Space: `O(V + E)` for adjacency list, bookkeeping arrays, and queue/stack.

## Practice questions (concept check)
1. Why does Topological Sort only work on DAGs?
2. In Kahn's algorithm, what does an indegree of `0` represent?
3. In DFS-based Topological Sort, why do we reverse postorder?
4. How does each method detect cycles?

<details>
<summary>Answers</summary>

1. A cycle creates contradictory ordering constraints, so no valid linear order exists.
2. It means the node currently has no unmet prerequisites and can be processed now.
3. Nodes are added after exploring descendants, so reversing gives prerequisite-before-dependent order.
4. Kahn's: processed node count < `V`; DFS: encountering a gray/visiting node.

</details>

## Practice questions (LeetCode)
1. [Course Schedule](https://leetcode.com/problems/course-schedule)
2. [Alien Dictionary](https://leetcode.com/problems/alien-dictionary)
3. [Sort Items by Groups Respecting Dependencies](https://leetcode.com/problems/sort-items-by-groups-respecting-dependencies)

## One thing that was confusing to me
At first, I was unsure why multiple valid answers can exist; I learned topological order is often not unique as long as all dependencies are respected.

## See also
- [Breadth-First Search (BFS)](./bfs.md)
- [Depth-First Search (DFS)](./dfs.md)
