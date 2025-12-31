# Binary Search Tree

> [!note] Description
> A Binary Search Tree (BST) is a node-based binary tree data structure which has the following properties:
> - The left subtree of a node contains only nodes with keys lesser than the node’s key.
> - The right subtree of a node contains only nodes with keys greater than the node’s key.
> - The left and right subtree each must also be a binary search tree.

## Operations

### 1. Insert element
- **Complexity**: $O(h)$ where $h$ is height of tree. $O(\log n)$ average, $O(n)$ worst.

### 2. Delete element
- **Complexity**: $O(h)$.

### 3. Update element
Delete old value, insert new value.
- **Complexity**: $O(h)$.

### 4. Search element
- **Complexity**: $O(h)$.

### 5. Sort the elements
In-order traversal gives sorted elements.
- **Complexity**: $O(n)$.

## Complexity Summary

| Operation | Time Complexity (Average) | Time Complexity (Worst) |
| :--- | :--- | :--- |
| Access | $O(\log n)$ | $O(n)$ |
| Search | $O(\log n)$ | $O(n)$ |
| Insertion | $O(\log n)$ | $O(n)$ |
| Deletion | $O(\log n)$ | $O(n)$ |

## Implementation Example (Python)

```python
class Node:
    def __init__(self, key):
        self.left = None
        self.right = None
        self.val = key

def insert(root, key):
    if root is None:
        return Node(key)
    else:
        if root.val < key:
            root.right = insert(root.right, key)
        else:
            root.left = insert(root.left, key)
    return root

def search(root, key):
    if root is None or root.val == key:
        return root
    if root.val < key:
        return search(root.right, key)
    return search(root.left, key)
```
