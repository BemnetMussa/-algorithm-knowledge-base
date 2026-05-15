# Dijkstra's Algorithm

## One-sentence definition
Dijkstra's algorithm finds the shortest path from a starting node to all other nodes in a weighted graph with **non‑negative** edge weights.

## When to use it (use cases)
- Finding the shortest driving distance between two locations on a map.
- Network routing protocols (OSPF uses a variant of Dijkstra).
- Social networks – shortest chain of connections.
- Video games – pathfinding for characters (often combined with A*).
- Solving problems like "minimum time to reach destination with weighted edges".

## Step-by-step explanation (plain language, no code first)

1. **Initialize** – Set the distance to the start node as 0, and to all other nodes as infinity. Keep a set/priority queue of unvisited nodes.

2. **Visit the closest unvisited node** – Start with the start node (distance 0). For that node, look at all its neighbors. Calculate the tentative distance to each neighbor via the current node. If it’s smaller than the previously recorded distance, update it.

3. **Mark node as visited** – Once you’ve processed all neighbors, mark the current node as visited (its distance is final).

4. **Repeat** – Pick the next unvisited node with the smallest known distance. Continue until you’ve visited the target node (or all reachable nodes).

> **Analogy:** You have a map with towns and roads (each road has a length). You mark the starting town with a post‑it that says “0 km”. Every time you arrive at a new town, you check all roads leaving it. If going through this town gives a shorter total distance to a neighbor, you update the neighbor’s post‑it. Always go to the unvisited town with the smallest number on its post‑it next.

## Templates (Python)

### Dijkstra using `heapq` (priority queue)
```python
import heapq

def dijkstra(graph, start):
    """
    graph: dict where graph[node] = [(neighbor, weight), ...]
    returns: dict of shortest distances from start to every node
    """
    distances = {node: float('inf') for node in graph}
    distances[start] = 0
    pq = [(0, start)]  # (distance, node)
    
    while pq:
        current_dist, current = heapq.heappop(pq)
        
        # Skip if we already found a better path
        if current_dist > distances[current]:
            continue
        
        for neighbor, weight in graph[current]:
            new_dist = current_dist + weight
            if new_dist < distances[neighbor]:
                distances[neighbor] = new_dist
                heapq.heappush(pq, (new_dist, neighbor))
    
    return distances
```

### Dijkstra to find shortest path to a single target (early stop)
```python
def dijkstra_target(graph, start, target):
    distances = {node: float('inf') for node in graph}
    distances[start] = 0
    pq = [(0, start)]
    
    while pq:
        current_dist, current = heapq.heappop(pq)
        
        if current == target:
            return current_dist  # early exit
        
        if current_dist > distances[current]:
            continue
        
        for neighbor, weight in graph[current]:
            new_dist = current_dist + weight
            if new_dist < distances[neighbor]:
                distances[neighbor] = new_dist
                heapq.heappush(pq, (new_dist, neighbor))
    
    return distances[target] if distances[target] != float('inf') else -1
```

### Example graph representation
```python
graph = {
    'A': [('B', 4), ('C', 2)],
    'B': [('C', 1), ('D', 5)],
    'C': [('D', 8), ('E', 10)],
    'D': [('E', 2)],
    'E': []
}
dist = dijkstra(graph, 'A')
print(dist)  # {'A': 0, 'B': 4, 'C': 2, 'D': 7, 'E': 9}
```

## Time & space complexity (Big O)
- **Time:** `O((V + E) log V)` with binary heap (using `heapq`), where V = vertices, E = edges.
- **Time (naive array):** `O(V²)` – fine for dense graphs.
- **Space:** `O(V)` for distances and priority queue.

## Practice questions (concept check)
1. Why does Dijkstra's algorithm fail with negative edge weights?
2. What data structure is best for the “unvisited node with smallest distance” step?
3. Does Dijkstra's algorithm work for undirected graphs? Why?

<details>
<summary>Answers</summary>

1. Because once a node is marked visited, its distance is assumed final. A negative edge could later provide a shorter path through a visited node, breaking the greedy assumption.

2. A priority queue (min‑heap). Using an array gives O(V²), using a heap gives O((V+E) log V).

3. Yes, it works fine. Treat each undirected edge as two directed edges (both directions) with the same weight.

</details>

## Practice questions (LeetCode/Codeforces)
1. [Network Delay Time](https://leetcode.com/problems/network-delay-time/) – classic Dijkstra.
2. [Path with Minimum Effort](https://leetcode.com/problems/path-with-minimum-effort/) – can use Dijkstra (treat effort as weight).
3. [Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/) – Bellman‑Ford / Dijkstra with state.
4. [Shortest Path Visiting All Nodes](https://leetcode.com/problems/shortest-path-visiting-all-nodes/) – harder, but uses Dijkstra + bitmask.

## One thing that was confusing to me
I used to think Dijkstra could be implemented with a simple queue (like BFS). But BFS works only for unit weights. With variable weights, you must always pick the node with the smallest current distance; that’s why a priority queue is essential. I learned the hard way when my simple queue gave wrong distances for a graph with weights 1 and 10.

## See also
- [Bellman‑Ford Algorithm](../graph/bellman-ford.md) – handles negative edges (slower).
- [A* Search](../graph/a-star.md) – heuristic‑guided Dijkstra.
- [Prim's MST](../graph/prims.md) – similar greedy algorithm for minimum spanning trees.

Want me to also create **Bellman‑Ford** (handles negative weights) or **Floyd‑Warshall** (all‑pairs) next? Or move to **Dynamic Programming** (0/1 knapsack)? Your call.
