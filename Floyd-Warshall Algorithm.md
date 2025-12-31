# Floyd-Warshall Algorithm

> [!note] Description
> The Floyd-Warshall algorithm is an algorithm for finding shortest paths in a weighted graph with positive or negative edge weights (but with no negative cycles). It finds the shortest paths between all pairs of vertices.

## Complexity
- **Time Complexity**: $O(V^3)$.
- **Space Complexity**: $O(V^2)$.

## Implementation (Python)

```python
INF = 99999

def floyd_warshall(graph, V):
    dist = list(map(lambda i: list(map(lambda j: j, i)), graph))

    for k in range(V):
        for i in range(V):
            for j in range(V):
                dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])

    return dist
```
