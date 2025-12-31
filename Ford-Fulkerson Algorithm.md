# Ford-Fulkerson Algorithm

> [!note] Description
> The Ford-Fulkerson method is a greedy algorithm that computes the maximum flow in a flow network.

## Intuition
The intuition is like sending water through a network of pipes. You want to send as much water as possible from a source to a sink. You look for any path that has available capacity (an "augmenting path"). If you find one, you push as much flow as that path allows (limited by the narrowest pipe). Pushing flow uses up capacity in the forward direction but creates "residual capacity" in the backward direction (allowing you to "undo" flow later if needed to find a better global solution). You keep finding paths and adding flow until no more paths exist.

## Complexity
- **Time Complexity**: $O(E \times f)$ where $f$ is max flow value.
- **Space Complexity**: $O(V + E)$.

## Implementation (Python)

```python
class Graph:
    def __init__(self, graph):
        self.graph = graph
        self.ROW = len(graph)

    def bfs(self, s, t, parent):
        visited = [False] * (self.ROW)
        queue = []
        queue.append(s)
        visited[s] = True
        parent[s] = -1

        while queue:
            u = queue.pop(0)
            for v, val in enumerate(self.graph[u]):
                if visited[v] == False and val > 0:
                    queue.append(v)
                    visited[v] = True
                    parent[v] = u
        return True if visited[t] else False

    def ford_fulkerson(self, source, sink):
        parent = [-1] * (self.ROW)
        max_flow = 0
        while self.bfs(source, sink, parent):
            path_flow = float("Inf")
            s = sink
            while(s != source):
                path_flow = min(path_flow, self.graph[parent[s]][s])
                s = parent[s]
            max_flow += path_flow
            v = sink
            while(v != source):
                u = parent[v]
                self.graph[u][v] -= path_flow
                self.graph[v][u] += path_flow
                v = parent[v]
        return max_flow
```
