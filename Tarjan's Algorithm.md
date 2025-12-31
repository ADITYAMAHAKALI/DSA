# Tarjan's Algorithm

> [!note] Description
> Tarjan's algorithm is an algorithm in graph theory for finding the strongly connected components (SCCs) of a directed graph.

## Complexity
- **Time Complexity**: $O(V + E)$.
- **Space Complexity**: $O(V)$.

## Implementation (Python)

```python
class Tarjan:
    def __init__(self, graph):
        self.graph = graph
        self.time = 0
        self.disc = {}
        self.low = {}
        self.stack_member = {}
        self.st = []
        self.sccs = []

    def scc_util(self, u):
        self.disc[u] = self.low[u] = self.time
        self.time += 1
        self.stack_member[u] = True
        self.st.append(u)

        for v in self.graph.get(u, []):
            if v not in self.disc:
                self.scc_util(v)
                self.low[u] = min(self.low[u], self.low[v])
            elif self.stack_member[v]:
                self.low[u] = min(self.low[u], self.disc[v])

        if self.low[u] == self.disc[u]:
            component = []
            while True:
                w = self.st.pop()
                self.stack_member[w] = False
                component.append(w)
                if u == w:
                    break
            self.sccs.append(component)

    def find_sccs(self):
        for i in self.graph:
            if i not in self.disc:
                self.scc_util(i)
        return self.sccs
```
