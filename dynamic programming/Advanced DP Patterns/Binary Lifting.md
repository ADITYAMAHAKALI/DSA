> [!note] Description
> Binary Lifting is a technique to perform queries on a path in a tree (like finding ancestors) in $O(\log N)$ time by precomputing ancestors at powers of 2 ($1, 2, 4, 8 \dots$).

## 1. K-th Ancestor of a Tree Node

### Intuition
You are given a tree with `n` nodes. Implement a function to find the `k`-th ancestor of a given node `node`.
We precompute `up[node][i]` which stores the $2^i$-th ancestor of `node`.
To find the `k`-th ancestor, we express `k` in binary. If the $i$-th bit of `k` is set, we jump $2^i$ steps up.

### Implementation (Preprocessing + Query)
```python
class TreeAncestor:

    def __init__(self, n: int, parent: List[int]):
        self.LOG = 20 # 2^20 > 50000
        self.up = [[-1] * self.LOG for _ in range(n)]

        # 2^0 ancestor is the parent
        for i in range(n):
            self.up[i][0] = parent[i]

        # DP to fill up table
        for j in range(1, self.LOG):
            for i in range(n):
                if self.up[i][j-1] != -1:
                    self.up[i][j] = self.up[self.up[i][j-1]][j-1]
                else:
                    self.up[i][j] = -1

    def getKthAncestor(self, node: int, k: int) -> int:
        for j in range(self.LOG):
            if k & (1 << j):
                node = self.up[node][j]
                if node == -1:
                    return -1
        return node
```

### Complexity
- **Time:** $O(N \log N)$ preprocessing, $O(\log N)$ query.
- **Space:** $O(N \log N)$.

---

## 2. Lowest Common Ancestor (LCA)

### Intuition
Find the lowest common ancestor of two nodes `u` and `v` in a tree.
1. Make `u` and `v` the same depth by lifting the deeper node.
2. If `u == v`, return `u`.
3. Lift both `u` and `v` upwards. If `up[u][i] != up[v][i]`, it means the LCA is higher, so we can jump up to `up[u][i]` and `up[v][i]`.
4. The parent of the node we land on is the LCA.

### Implementation
```python
class Solution:
    def __init__(self, n, adj, root=0):
        self.LOG = 20
        self.depth = [0] * n
        self.up = [[-1] * self.LOG for _ in range(n)]
        self.adj = adj

        self.dfs(root, -1, 0)

    def dfs(self, u, p, d):
        self.depth[u] = d
        self.up[u][0] = p
        for i in range(1, self.LOG):
            if self.up[u][i-1] != -1:
                self.up[u][i] = self.up[self.up[u][i-1]][i-1]

        for v in self.adj[u]:
            if v != p:
                self.dfs(v, u, d + 1)

    def get_lca(self, u, v):
        if self.depth[u] < self.depth[v]:
            u, v = v, u

        # Bring u to same depth as v
        diff = self.depth[u] - self.depth[v]
        for i in range(self.LOG):
            if (diff >> i) & 1:
                u = self.up[u][i]

        if u == v: return u

        for i in range(self.LOG - 1, -1, -1):
            if self.up[u][i] != self.up[v][i]:
                u = self.up[u][i]
                v = self.up[v][i]

        return self.up[u][0]
```

### Complexity
- **Time:** $O(N \log N)$ build, $O(\log N)$ query.
- **Space:** $O(N \log N)$.

---

## 3. Maximum Edge in Path (Path Aggregation)

### Intuition
Given a weighted tree, find the maximum weight edge on the path between two nodes `u` and `v`.
We can augment the binary lifting table to store `max_edge[u][i]`, which is the max edge weight on the path from `u` to `up[u][i]`.
`max_edge[u][i] = max(max_edge[u][i-1], max_edge[up[u][i-1]][i-1])`.

### Implementation (Snippet)
```python
# Inside DFS or Init
# self.max_w[u][i] stores max edge weight for 2^i jump
for i in range(1, self.LOG):
    if self.up[u][i-1] != -1:
        self.up[u][i] = self.up[self.up[u][i-1]][i-1]
        self.max_w[u][i] = max(self.max_w[u][i-1], self.max_w[self.up[u][i-1]][i-1])

# Query logic
# When lifting u to match depth of v, update current_max with max_w[u][i]
# When lifting both u and v, update current_max with max(max_w[u][i], max_w[v][i])
```

### Complexity
- **Time:** $O(N \log N)$ build, $O(\log N)$ query.
- **Space:** $O(N \log N)$.
