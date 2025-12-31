# Breadth-First Search (BFS)

> [!note] Description
> Breadth First Search or BFS for a Graph is similar to Breadth First Traversal of a tree. The only catch here is, unlike trees, graphs may contain cycles, so we may come to the same node again. To avoid processing a node more than once, we use a boolean visited array.

## Intuition
The intuition behind BFS is to explore the graph in "waves" or layers spreading out from the starting node. Imagine dropping a stone in a pond; the ripples move outward in concentric circles. BFS visits all immediate neighbors (distance 1) before moving to neighbors of neighbors (distance 2), and so on. This "closest-first" property makes it ideal for finding the shortest path in unweighted graphs.

## Complexity
- **Time Complexity**: $O(V + E)$ where $V$ is vertices and $E$ is edges.
- **Space Complexity**: $O(V)$.

## Implementation (Python)

```python
from collections import deque

def bfs(graph, start):
    visited = set()
    queue = deque([start])
    visited.add(start)

    while queue:
        vertex = queue.popleft()
        print(vertex, end=" ")

        for neighbour in graph[vertex]:
            if neighbour not in visited:
                visited.add(neighbour)
                queue.append(neighbour)
```
