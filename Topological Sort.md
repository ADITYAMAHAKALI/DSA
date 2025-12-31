# Topological Sort

> [!note] Description
> Topological sort for Directed Acyclic Graph (DAG) is a linear ordering of vertices such that for every directed edge $uv$, vertex $u$ comes before $v$ in the ordering.

## Complexity
- **Time Complexity**: $O(V + E)$.
- **Space Complexity**: $O(V)$.

## Implementation (Python)

```python
def topological_sort_util(v, visited, stack, graph):
    visited[v] = True
    if v in graph:
        for i in graph[v]:
            if not visited[i]:
                topological_sort_util(i, visited, stack, graph)
    stack.insert(0, v)

def topological_sort(graph, V):
    visited = [False] * V
    stack = []
    for i in range(V):
        if not visited[i]:
            topological_sort_util(i, visited, stack, graph)
    return stack
```
