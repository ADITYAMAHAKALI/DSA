# Graph

> [!note] Description
> A Graph is a non-linear data structure consisting of nodes and edges. The nodes are sometimes also referred to as vertices and the edges are lines or arcs that connect any two nodes in the graph.

## Operations

### 1. Insert element (Add Vertex/Edge)
- **Complexity**: $O(1)$ for vertex (adjacency list), $O(1)$ for edge (adjacency list). $O(V^2)$ for adjacency matrix vertex addition (reallocation).

### 2. Delete element (Remove Vertex/Edge)
- **Complexity**: $O(V+E)$ (adjacency list) to remove vertex and incident edges. $O(E)$ or $O(1)$ for edge.

### 3. Update element
- **Complexity**: $O(1)$ if updating value at vertex/edge.

### 4. Search element (Traversal)
- **Complexity**: $O(V+E)$ (BFS/DFS).

### 5. Sort the elements
Topological Sort for DAGs.
- **Complexity**: $O(V+E)$.

## Complexity Summary

| Operation | Adjacency List | Adjacency Matrix |
| :--- | :--- | :--- |
| Add Vertex | $O(1)$ | $O(V^2)$ |
| Add Edge | $O(1)$ | $O(1)$ |
| Remove Vertex | $O(V+E)$ | $O(V^2)$ |
| Remove Edge | $O(E)$ | $O(1)$ |
| Query | $O(V)$ | $O(1)$ |

## Implementation Example (Python - Adjacency List)

```python
class Graph:
    def __init__(self):
        self.graph = {}

    # 1. Insert (Add Edge/Vertex)
    def add_edge(self, u, v):
        if u not in self.graph:
            self.graph[u] = []
        self.graph[u].append(v)

    # 4. Search (BFS)
    def bfs(self, start):
        visited = set()
        queue = [start]
        visited.add(start)
        while queue:
            vertex = queue.pop(0)
            print(vertex, end=" ")
            for neighbour in self.graph.get(vertex, []):
                if neighbour not in visited:
                    visited.add(neighbour)
                    queue.append(neighbour)

    # 2. Delete (Remove Edge)
    def remove_edge(self, u, v):
        if u in self.graph and v in self.graph[u]:
            self.graph[u].remove(v)

    # 3. Update (Update edge target - rarely done, usually remove and add)
    def update_edge(self, u, old_v, new_v):
        self.remove_edge(u, old_v)
        self.add_edge(u, new_v)

    # 5. Sort (Topological Sort - simplified)
    def topological_sort(self):
        # Implementation depends on DAG property
        pass
```
