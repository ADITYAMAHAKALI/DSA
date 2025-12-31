# Disjoint Set (Union-Find)

> [!note] Description
> A Disjoint-Set data structure is a data structure that keeps track of a set of elements partitioned into a number of disjoint (non-overlapping) subsets. A union-find algorithm is an algorithm that performs two useful operations on such a data structure:
> - **Find**: Determine which subset a particular element is in.
> - **Union**: Join two subsets into a single subset.

## Operations

### 1. Insert element (Make Set)
Creates a new set with one element.
- **Complexity**: $O(1)$.

### 2. Delete element
Not typically supported.

### 3. Update element
Not typically supported.

### 4. Search element (Find)
Find representative of the set.
- **Complexity**: $O(\alpha(n))$ (Inverse Ackermann function, nearly constant).

### 5. Sort the elements
Not applicable. Use to group elements.

### Union Operation
Unites two sets.
- **Complexity**: $O(\alpha(n))$.

## Complexity Summary

| Operation | Time Complexity |
| :--- | :--- |
| Union | $O(\alpha(n))$ |
| Find | $O(\alpha(n))$ |

## Implementation Example (Python)

```python
class DisjointSet:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n

    def find(self, i):
        if self.parent[i] != i:
            self.parent[i] = self.find(self.parent[i]) # Path compression
        return self.parent[i]

    def union(self, i, j):
        root_i = self.find(i)
        root_j = self.find(j)
        if root_i != root_j:
            # Union by rank
            if self.rank[root_i] < self.rank[root_j]:
                self.parent[root_i] = root_j
            elif self.rank[root_i] > self.rank[root_j]:
                self.parent[root_j] = root_i
            else:
                self.parent[root_j] = root_i
                self.rank[root_i] += 1
```
