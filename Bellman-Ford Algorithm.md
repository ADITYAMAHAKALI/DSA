# Bellman-Ford Algorithm

> [!note] Description
> The Bellman-Ford algorithm is an algorithm that computes shortest paths from a single source vertex to all of the other vertices in a weighted digraph. It is slower than Dijkstra's algorithm for the same problem, but more versatile, as it is capable of handling graphs in which some of the edge weights are negative numbers.

## Intuition
The intuition is based on the principle of relaxation. We repeatedly "relax" all edges, meaning we check if we can improve the shortest path estimate to a node $v$ by going through node $u$. In a graph with $V$ vertices, the shortest path can have at most $V-1$ edges. Therefore, if we relax all edges $V-1$ times, we guarantee finding the shortest paths. If we can still improve a path after $V-1$ iterations, it implies the existence of a negative weight cycle.

## Complexity
- **Time Complexity**: $O(V \times E)$.
- **Space Complexity**: $O(V)$.

## Implementation (Python)

```python
class Graph:
    def __init__(self, vertices):
        self.V = vertices
        self.graph = []

    def add_edge(self, u, v, w):
        self.graph.append([u, v, w])

    def bellman_ford(self, src):
        dist = [float("Inf")] * self.V
        dist[src] = 0

        for _ in range(self.V - 1):
            for u, v, w in self.graph:
                if dist[u] != float("Inf") and dist[u] + w < dist[v]:
                    dist[v] = dist[u] + w

        for u, v, w in self.graph:
            if dist[u] != float("Inf") and dist[u] + w < dist[v]:
                print("Graph contains negative weight cycle")
                return

        return dist
```
