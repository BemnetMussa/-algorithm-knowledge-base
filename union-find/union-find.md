# Union-Find (Disjoint Set Union - DSU)

## One-sentence definition
Union-Find is a data structure that efficiently tracks connected components by supporting fast `find` (component lookup) and `union` (merge components) operations.

## When to use it (use cases)
- Dynamic connectivity problems in undirected graphs.
- Cycle detection in undirected graphs.
- Counting connected components.
- Kruskal's Minimum Spanning Tree algorithm.
- Grouping elements by equivalence relations.

## Step-by-step explanation
1. Start with each element as its own parent (each node is its own set).
2. `find(x)` returns the representative (root) of `x`'s set.
3. `union(a, b)` merges sets if their representatives differ.
4. Use **path compression** in `find` to flatten trees.
5. Use **union by rank/size** to keep trees shallow.

## Visual intuition (ASCII)

### Before unions
```text
0   1   2   3   4
```

### After union(0,1), union(1,2), union(3,4)
```text
0 - 1 - 2      3 - 4
```

### Parent array idea
```text
parent[i] = representative parent of i
find(i) follows parent links until root
```

## Templates (Python)
### 1) DSU with path compression + union by rank
Use when: default DSU template for most problems.
Input: number of nodes `n`, `union(a,b)`, `find(x)`.
Output: merged sets + connectivity queries.
```python
class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n
        self.components = n

    def find(self, x):
        # Path compression: flatten tree for near O(1) amortized finds.
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, a, b):
        ra = self.find(a)
        rb = self.find(b)
        if ra == rb:
            return False  # already same component

        # Union by rank: attach shorter tree under taller tree.
        if self.rank[ra] < self.rank[rb]:
            ra, rb = rb, ra
        self.parent[rb] = ra
        if self.rank[ra] == self.rank[rb]:
            self.rank[ra] += 1

        self.components -= 1
        return True
```

### 2) DSU with union by size
Use when: you want quick access to component sizes.
Input: number of nodes `n`.
Output: supports `size_of(x)` queries.
```python
class DSUSize:
    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n

    def find(self, x):
        while self.parent[x] != x:
            # Path halving (iterative compression).
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def union(self, a, b):
        ra = self.find(a)
        rb = self.find(b)
        if ra == rb:
            return False

        # Invariant: attach smaller component to larger one.
        if self.size[ra] < self.size[rb]:
            ra, rb = rb, ra
        self.parent[rb] = ra
        self.size[ra] += self.size[rb]
        return True

    def size_of(self, x):
        return self.size[self.find(x)]
```

### 3) Count connected components from edges
Use when: you are given `n` and edge list, need number of components.
Input: `n`, list of undirected edges.
Output: integer component count.
```python
def count_components(n, edges):
    dsu = DSU(n)
    for u, v in edges:
        dsu.union(u, v)
    return dsu.components
```

### 4) Detect cycle in an undirected graph
Use when: detect if any edge connects already-connected vertices.
Input: `n`, list of undirected edges.
Output: boolean (`True` if cycle exists).
```python
def has_cycle_undirected(n, edges):
    dsu = DSU(n)
    for u, v in edges:
        if not dsu.union(u, v):
            return True
    return False
```

## Time & space complexity (Big O)
- `find` with compression: amortized near `O(1)` (formally `O(alpha(n))`).
- `union` with rank/size: amortized near `O(1)`.
- For `m` operations on `n` nodes: `O((n + m) * alpha(n))`.
- Space: `O(n)`.

## Practice questions (concept check)
1. Why do we need both path compression and union by rank/size?
2. Why does `union(a, b)` returning `False` help with cycle detection?
3. Why is DSU best for undirected connectivity but not direct shortest-path problems?

<details>
<summary>Answers</summary>

1. Both heuristics keep trees shallow, making operations very fast in practice.
2. If representatives are already equal, adding that edge closes a cycle.
3. DSU tracks component membership, not path length or path structure.

</details>

## Practice questions (LeetCode/Codeforces)
1. [Number of Provinces](https://leetcode.com/problems/number-of-provinces)
2. [Redundant Connection](https://leetcode.com/problems/redundant-connection)
3. [Accounts Merge](https://leetcode.com/problems/accounts-merge)
4. [Most Stones Removed with Same Row or Column](https://leetcode.com/problems/most-stones-removed-with-same-row-or-column)
5. [Satisfiability of Equality Equations](https://leetcode.com/problems/satisfiability-of-equality-equations)

## One thing that was confusing to me
At first, I thought DSU could answer path queries, but I learned it only answers whether nodes belong to the same component (and can track component metadata like size).

## See also
- [Depth-First Search (DFS)](../graph/dfs.md)
- [Heap](../heap/heap.md)
