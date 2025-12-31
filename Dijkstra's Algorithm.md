# Dijkstra's Algorithm

> [!note] Description
> Dijkstra's algorithm is an algorithm for finding the shortest paths between nodes in a graph, which may represent, for example, road networks. It was conceived by computer scientist Edsger W. Dijkstra in 1956.

## Intuition
Dijkstra's algorithm is a "greedy" approach to finding the shortest path. It's like spreading water from a source: it flows to the nearest places first. We always expand the shortest known path. By maintaining a set of visited nodes and tentatively assigned shortest distances for unvisited nodes, we repeatedly select the unvisited node with the smallest tentative distance, visit it, and update the distances to its neighbors. This ensures that when we finally visit a node, we've found the absolute shortest path to it (assuming non-negative weights).

## Complexity
- **Time Complexity**: $O(E + V \log V)$ using Min-Priority Queue.
- **Space Complexity**: $O(V)$.

## Implementation (Python)

```python
import heapq

def dijkstra(graph, start):
    distances = {node: float('infinity') for node in graph}
    distances[start] = 0
    pq = [(0, start)]

    while pq:
        current_distance, current_node = heapq.heappop(pq)

        if current_distance > distances[current_node]:
            continue

        for neighbor, weight in graph[current_node].items():
            distance = current_distance + weight
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                heapq.heappush(pq, (distance, neighbor))

    return distances
```
