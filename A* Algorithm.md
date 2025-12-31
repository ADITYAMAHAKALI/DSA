# A* Algorithm

> [!note] Description
> A* (pronounced "A-star") is a graph traversal and path search algorithm, which is often used in many fields of computer science due to its completeness, optimality, and optimal efficiency. It uses heuristics to guide the search.

## Complexity
- **Time Complexity**: Depends on the heuristic. In worst case, exponential.
- **Space Complexity**: $O(V)$ (keeps all generated nodes in memory).

## Implementation (Python)

```python
import heapq

def a_star(graph, start, goal, h):
    # Priority Queue stores tuples of (f_score, node)
    open_set = []
    heapq.heappush(open_set, (0, start))

    came_from = {}

    # Cost from start to node
    g_score = {node: float('inf') for node in graph}
    g_score[start] = 0

    # Estimated total cost from start to goal through node
    f_score = {node: float('inf') for node in graph}
    f_score[start] = h(start)

    # Keep track of visited nodes to avoid reprocessing (since we use lazy deletion)
    # Actually, standard A* re-opens closed nodes if a better path is found,
    # but with consistent heuristic (monotonic), we don't need to re-open.
    # Here we assume consistent heuristic.

    while open_set:
        # Pop the node with the lowest f_score
        current_f, current = heapq.heappop(open_set)

        if current == goal:
            return reconstruct_path(came_from, current)

        # Lazy deletion: if we popped a worse path to 'current', ignore it
        if current_f > f_score[current]:
            continue

        for neighbor, weight in graph[current].items():
            tentative_g_score = g_score[current] + weight
            if tentative_g_score < g_score[neighbor]:
                came_from[neighbor] = current
                g_score[neighbor] = tentative_g_score
                f_score[neighbor] = tentative_g_score + h(neighbor)

                # Push to heap even if it's already there (duplicate entries are handled by the check above)
                heapq.heappush(open_set, (f_score[neighbor], neighbor))

    return False

def reconstruct_path(came_from, current):
    total_path = [current]
    while current in came_from:
        current = came_from[current]
        total_path.append(current)
    return total_path[::-1]
```
