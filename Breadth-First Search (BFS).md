# Breadth-First Search (BFS)

> [!note] Description
> Breadth First Search or BFS for a Graph is similar to Breadth First Traversal of a tree. The only catch here is, unlike trees, graphs may contain cycles, so we may come to the same node again. To avoid processing a node more than once, we use a boolean visited array.

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
