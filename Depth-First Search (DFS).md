# Depth-First Search (DFS)

> [!note] Description
> Depth First Search or DFS for a Graph is similar to Depth First Traversal of a tree. The only catch here is, unlike trees, graphs may contain cycles, a node may be visited twice. To avoid processing a node more than once, use a boolean visited array.

## Intuition
The intuition behind DFS is to go as deep as possible down one path before backing up. Imagine exploring a maze: you walk down a corridor until you hit a dead end, then you backtrack to the last junction and try a different path. This "plunge depth-first" strategy is naturally implemented using recursion (a system stack) or an explicit stack data structure.

## Complexity
- **Time Complexity**: $O(V + E)$.
- **Space Complexity**: $O(V)$.

## Implementation (Python)

```python
def dfs(graph, start, visited=None):
    if visited is None:
        visited = set()
    visited.add(start)
    print(start, end=" ")
    for next in graph[start] - visited:
        dfs(graph, next, visited)
    return visited

graph = {'0': set(['1', '2']),
         '1': set(['0', '3', '4']),
         '2': set(['0']),
         '3': set(['1']),
         '4': set(['2', '3'])}

dfs(graph, '0')
```
