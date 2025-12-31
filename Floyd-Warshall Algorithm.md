# Floyd-Warshall Algorithm

> [!note] Description
> The Floyd-Warshall algorithm is an algorithm for finding shortest paths in a weighted graph with positive or negative edge weights (but with no negative cycles). It finds the shortest paths between all pairs of vertices.

## Intuition
The intuition is based on dynamic programming. We consider all intermediate vertices $k$ from $1$ to $V$. For every pair of vertices $(i, j)$, we check if the path from $i$ to $j$ going through $k$ is shorter than the currently known path. We update the shortest path $dist[i][j]$ to be the minimum of $dist[i][j]$ and $dist[i][k] + dist[k][j]$. Essentially, we are building up the solution by iteratively allowing more nodes to act as intermediates.

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
