# Topological Sort

> [!note] Description
> Topological sort for Directed Acyclic Graph (DAG) is a linear ordering of vertices such that for every directed edge $uv$, vertex $u$ comes before $v$ in the ordering.

## Intuition
The intuition is organizing dependencies. Imagine a task list where some tasks cannot start until others are finished (like "put on socks" before "put on shoes"). Topological sort gives you a valid linear sequence to perform all tasks. It works by ensuring that if there is a dependency $A \to B$, $A$ will always appear before $B$ in the sorted list. This is only possible if there are no cycles (you can't have A wait for B and B wait for A).

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
