# Kruskal's Algorithm

## One-sentence definition
Kruskal's algorithm finds a Minimum Spanning Tree (MST) by repeatedly adding the smallest edge that connects two different components, avoiding cycles, until all vertices are connected.

## When to use it (use cases)
- Building a cost‑efficient network (e.g., roads, cables, pipelines) – connect all points with minimum total length.
- Clustering algorithms (single‑linkage clustering).
- Approximations for NP‑hard problems (e.g., traveling salesman problem).
- When edges are few (sparse graphs) – Kruskal performs well.

## Step-by-step explanation (plain language, no code first)

1. **Sort all edges** by weight (smallest to largest).

2. **Initialize** – each vertex starts as its own component (disjoint set / union‑find).

3. **Process edges one by one**:
   - Take the smallest edge.
   - If its two endpoints belong to **different components**, add it to the MST and merge the two components.
   - If they are already in the same component, skip the edge (it would create a cycle).

4. **Stop** when you have added V−1 edges (V = number of vertices).

> **Analogy:** You have a list of roads with construction costs, and you want to connect all cities with the cheapest total cost. Start with the cheapest road. Add roads one by one, but never build a road that connects two cities already indirectly connected (that would be redundant). Stop when every city is reachable from any other.

## Templates (Python)

### Kruskal with Union‑Find (Disjoint Set Union)
```python
class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n
    
    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]
    
    def union(self, x, y):
        xr, yr = self.find(x), self.find(y)
        if xr == yr:
            return False
        if self.rank[xr] < self.rank[yr]:
            self.parent[xr] = yr
        elif self.rank[xr] > self.rank[yr]:
            self.parent[yr] = xr
        else:
            self.parent[yr] = xr
            self.rank[xr] += 1
        return True

def kruskal(n, edges):
    """
    n: number of vertices (0..n-1)
    edges: list of (weight, u, v)
    returns: (total_weight, mst_edges)
    """
    edges.sort()  # sort by weight
    dsu = DSU(n)
    total = 0
    mst_edges = []
    for w, u, v in edges:
        if dsu.union(u, v):
            total += w
            mst_edges.append((u, v, w))
            if len(mst_edges) == n - 1:
                break
    return total, mst_edges
```

### Example usage
```python
edges = [
    (1, 0, 1), (3, 0, 2), (2, 1, 2), (4, 1, 3), (5, 2, 3)
]
total, mst = kruskal(4, edges)
print(total)  # 1+2+4 = 7
print(mst)    # [(0,1,1), (1,2,2), (1,3,4)]
```

## Time & space complexity (Big O)
- **Time:** `O(E log E)` from sorting edges + near‑constant union‑find operations.
- **Space:** `O(V + E)` for storing edges and DSU.

## Practice questions (concept check)
1. Why does Kruskal never create a cycle?
2. What happens if the graph is disconnected?
3. Why sort edges by weight?

<details>
<summary>Answers</summary>

1. The algorithm only adds an edge if its endpoints are in different components (checked via union‑find). Adding edges within the same component would create a cycle.

2. Kruskal will still run, but it will add fewer than V−1 edges. The result is a minimum spanning forest (an MST for each connected component).

3. To greedily take the cheapest edge first. This ensures the final tree has the smallest possible total weight (optimal substructure property).

</details>

## Practice questions (LeetCode/Codeforces)
1. [Min Cost to Connect All Points](https://leetcode.com/problems/min-cost-to-connect-all-points/) – build graph and apply Kruskal.
2. [Connecting Cities With Minimum Cost](https://leetcode.com/problems/connecting-cities-with-minimum-cost/) – classic MST.
3. [Find Critical and Pseudo‑Critical Edges in MST](https://leetcode.com/problems/find-critical-and-pseudo-critical-edges-in-mst/) – harder, builds on Kruskal.
4. [Kruskal's MST on Codeforces](https://codeforces.com/problemset/problem/1108/F) – MST with unique weights.

## One thing that was confusing to me
I thought any greedy approach would fail for MST. But Kruskal works because of the cut property: the smallest edge crossing any cut belongs to some MST. Sorting edges and adding them if they connect different components respects this property globally.

## See also
- [Prim's Algorithm](../graph/prims.md) – another MST algorithm (different approach).
- [Union‑Find (Disjoint Set)](../union-find/union-find.md) – essential for Kruskal.
- [Minimum Spanning Tree concepts](../graph/mst.md).
