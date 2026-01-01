> [!note] Description
> Bitmasking DP uses a bitmask (an integer) to represent the state of a set of items (e.g., visited nodes, selected items) when $N$ is small ($\le 20$).

## 1. Shortest Path Visiting All Nodes

### Intuition
You have an undirected, connected graph of `n` nodes labeled from `0` to `n - 1`. You are given an array `graph` where `graph[i]` is a list of all the nodes connected with node `i` by an edge.
Return the length of the shortest path that visits every node. You may start and stop at any node, you may revisit nodes multiple times, and you may reuse edges.
Since $N \le 12$, we can use a bitmask to track visited nodes.
State: `(mask, u)` where `mask` is the set of visited nodes and `u` is the current node.
This is BFS on the state space `(mask, u)`.

### BFS Approach (Standard for Shortest Path)
```python
class Solution:
    def shortestPathLength(self, graph: List[List[int]]) -> int:
        n = len(graph)
        if n == 1: return 0

        # Queue stores (u, mask, dist)
        queue = collections.deque()
        visited = set()

        for i in range(n):
            queue.append((i, 1 << i, 0))
            visited.add((i, 1 << i))

        final_mask = (1 << n) - 1

        while queue:
            u, mask, dist = queue.popleft()

            if mask == final_mask:
                return dist

            for v in graph[u]:
                new_mask = mask | (1 << v)
                if (v, new_mask) not in visited:
                    visited.add((v, new_mask))
                    queue.append((v, new_mask, dist + 1))

        return -1
```

### Complexity
- **Time:** $O(N \cdot 2^N)$
- **Space:** $O(N \cdot 2^N)$

---

## 2. Travelling Salesman Problem (Held-Karp)

### Intuition
Given a list of cities and the distances between each pair of cities, what is the shortest possible route that visits each city exactly once and returns to the origin city?
This is an NP-hard problem, but for small $N$ ($\approx 20$), we can solve it in $O(N^2 2^N)$.
State: `dp[mask][i]` = min cost to visit nodes in `mask`, ending at `i`.
Transition: `dp[mask][i] = min(dp[mask ^ (1<<i)][j] + dist[j][i])` for all `j` in `mask`.

### Bottom-Up (Tabulation)
```python
class Solution:
    def tsp(self, dist: List[List[int]]) -> int:
        n = len(dist)
        final_mask = (1 << n) - 1

        # dp[mask][i]
        dp = [[float('inf')] * n for _ in range(1 << n)]

        # Base case: start at node 0
        dp[1][0] = 0

        for mask in range(1, 1 << n):
            for i in range(n):
                # If node i is in mask
                if mask & (1 << i):
                    prev_mask = mask ^ (1 << i)
                    if prev_mask == 0: continue

                    for j in range(n):
                        if prev_mask & (1 << j):
                            dp[mask][i] = min(dp[mask][i], dp[prev_mask][j] + dist[j][i])

        # Return to start (0)
        ans = float('inf')
        for i in range(1, n):
            ans = min(ans, dp[final_mask][i] + dist[i][0])

        return ans
```

### Complexity
- **Time:** $O(N^2 \cdot 2^N)$
- **Space:** $O(N \cdot 2^N)$

---

## 3. Partition to K Equal Sum Subsets

### Intuition
Given an integer array `nums` and an integer `k`, return `true` if it is possible to divide this array into `k` non-empty subsets whose sums are all equal.
Target sum per subset = `sum(nums) / k`.
State: `dp[mask]` represents the remainder of the current subset sum modulo `target`. If `dp[mask] == -1`, it means this state is invalid. If `dp[mask] == 0`, it means we just completed a subset.

### Top-Down (Memoization)
```python
class Solution:
    def canPartitionKSubsets(self, nums: List[int], k: int) -> bool:
        total = sum(nums)
        if total % k != 0: return False
        target = total // k
        n = len(nums)
        nums.sort(reverse=True) # Optimization
        memo = {}

        def backtrack(mask, current_sum):
            if mask == (1 << n) - 1:
                return True
            if mask in memo:
                return memo[mask]

            for i in range(n):
                if not (mask & (1 << i)):
                    if current_sum + nums[i] <= target:
                        # If adding nums[i] completes a subset, reset current_sum to 0
                        new_sum = (current_sum + nums[i]) % target
                        if backtrack(mask | (1 << i), new_sum):
                            memo[mask] = True
                            return True

            memo[mask] = False
            return False

        return backtrack(0, 0)
```

### Complexity
- **Time:** $O(N \cdot 2^N)$
- **Space:** $O(2^N)$
