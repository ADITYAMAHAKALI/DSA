# Segment Tree

> [!note] Description
> A Segment Tree is a tree data structure used for storing information about intervals or segments. It allows querying which of the stored segments contain a given point. It is basically a static structure which needs a lot of memory.

## Operations

### 1. Insert element (Build)
- **Complexity**: $O(n)$.

### 2. Delete element
Not typical. Rebuild or Update.

### 3. Update element (Point Update)
- **Complexity**: $O(\log n)$.

### 4. Search element (Range Query)
- **Complexity**: $O(\log n)$.

### 5. Sort the elements
Not applicable.

## Complexity Summary

| Operation | Time Complexity |
| :--- | :--- |
| Build | $O(n)$ |
| Update | $O(\log n)$ |
| Query | $O(\log n)$ |
| Space | $O(n)$ |

## Implementation Example (Python)

```python
# Simple Sum Segment Tree
def build(arr, tree, node, start, end):
    if start == end:
        tree[node] = arr[start]
    else:
        mid = (start + end) // 2
        build(arr, tree, 2*node, start, mid)
        build(arr, tree, 2*node+1, mid+1, end)
        tree[node] = tree[2*node] + tree[2*node+1]

def update(arr, tree, node, start, end, idx, val):
    if start == end:
        arr[idx] = val
        tree[node] = val
    else:
        mid = (start + end) // 2
        if start <= idx <= mid:
            update(arr, tree, 2*node, start, mid, idx, val)
        else:
            update(arr, tree, 2*node+1, mid+1, end, idx, val)
        tree[node] = tree[2*node] + tree[2*node+1]

def query(tree, node, start, end, l, r):
    if r < start or end < l:
        return 0
    if l <= start and end <= r:
        return tree[node]
    mid = (start + end) // 2
    p1 = query(tree, 2*node, start, mid, l, r)
    p2 = query(tree, 2*node+1, mid+1, end, l, r)
    return p1 + p2
```
