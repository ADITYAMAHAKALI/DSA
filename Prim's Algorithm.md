# Prim's Algorithm

> [!note] Description
> Prim's algorithm is a greedy algorithm that finds a minimum spanning tree for a weighted undirected graph. It operates by building this tree one vertex at a time, from an arbitrary starting vertex, at each step adding the cheapest possible connection from the tree to another vertex.

## Complexity
- **Time Complexity**: $O(E + V \log V)$ with Min-Heap.
- **Space Complexity**: $O(V)$.

## Implementation (Python)

```python
import heapq

def prim_mst(graph, start_node):
    mst = []
    visited = set([start_node])
    edges = [
        (cost, start_node, to)
        for to, cost in graph[start_node].items()
    ]
    heapq.heapify(edges)

    while edges:
        cost, frm, to = heapq.heappop(edges)
        if to not in visited:
            visited.add(to)
            mst.append((frm, to, cost))
            for next_to, next_cost in graph[to].items():
                if next_to not in visited:
                    heapq.heappush(edges, (next_cost, to, next_to))

    return mst
```
