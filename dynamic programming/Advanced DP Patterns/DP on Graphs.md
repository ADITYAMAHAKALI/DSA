> [!note] Description
> DP on Graphs extends standard graph algorithms (BFS/DFS/Dijkstra) with DP states to count paths or optimize constrained movements.

## 1. Cheapest Flights Within K Stops

### Intuition
Find the cheapest price from `src` to `dst` with at most `k` stops.
This is Bellman-Ford or SPFA.
State: `dp[i][u]` = min cost to reach node `u` using exactly `i` stops (or edges).
Or just `dp[u]` for `k` stops, updating it `k+1` times.

### Bottom-Up (Tabulation / Bellman-Ford)
```python
class Solution:
    def findCheapestPrice(self, n: int, flights: List[List[int]], src: int, dst: int, k: int) -> int:
        # Distance array
        prices = [float('inf')] * n
        prices[src] = 0

        # Iterate k + 1 times (k stops = k+1 flights)
        for i in range(k + 1):
            temp_prices = list(prices)
            for u, v, w in flights:
                if prices[u] != float('inf'):
                    if prices[u] + w < temp_prices[v]:
                        temp_prices[v] = prices[u] + w
            prices = temp_prices

        return prices[dst] if prices[dst] != float('inf') else -1
```

### Complexity
- **Time:** $O(K \cdot E)$
- **Space:** $O(N)$

---

## 2. Count Paths in DAG (Source to Target)

### Intuition
Given a DAG, count paths from `0` to `n-1`.
Simple Top-Down DP.

### Top-Down (Memoization)
```python
class Solution:
    def allPathsSourceTarget(self, graph: List[List[int]]) -> List[List[int]]:
        # This standard problem asks for the paths themselves, but let's just count them for DP context.
        # If we need to count:
        target = len(graph) - 1
        memo = {}

        def count_paths(u):
            if u == target:
                return 1
            if u in memo:
                return memo[u]

            total = 0
            for v in graph[u]:
                total += count_paths(v)

            memo[u] = total
            return total

        return count_paths(0)

    # Note: Actual LeetCode 797 asks for the LIST of paths, which is backtracking, not just DP.
```

### Complexity
- **Time:** $O(V+E)$
- **Space:** $O(V)$

---

## 3. Find the City With the Smallest Number of Neighbors at a Threshold Distance

### Intuition
Floyd-Warshall Algorithm (All-Pairs Shortest Path).
Find shortest paths between all pairs `(i, j)`. Then for each city `i`, count `j` such that `dist[i][j] <= threshold`.

### Bottom-Up (Floyd-Warshall)
```python
class Solution:
    def findTheCity(self, n: int, edges: List[List[int]], distanceThreshold: int) -> int:
        dist = [[float('inf')] * n for _ in range(n)]

        for i in range(n):
            dist[i][i] = 0

        for u, v, w in edges:
            dist[u][v] = w
            dist[v][u] = w

        # Floyd-Warshall
        for k in range(n):
            for i in range(n):
                for j in range(n):
                    dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])

        min_reachable = float('inf')
        ans = -1

        for i in range(n):
            count = sum(1 for j in range(n) if dist[i][j] <= distanceThreshold)
            # We want city with smallest number of neighbors
            # If tie, greatest index
            if count <= min_reachable:
                min_reachable = count
                ans = i

        return ans
```

### Complexity
- **Time:** $O(N^3)$
- **Space:** $O(N^2)$
