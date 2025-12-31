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

# 1. Insert
def insert(root, key):
    if root is None:
        return Node(key)
    else:
        if root.val < key:
            root.right = insert(root.right, key)
        else:
            root.left = insert(root.left, key)
    return root

# 4. Search
def search(root, key):
    if root is None or root.val == key:
        return root
    if root.val < key:
        return search(root.right, key)
    return search(root.left, key)

# 2. Delete
def deleteNode(root, key):
    if root is None: return root
    if key < root.val:
        root.left = deleteNode(root.left, key)
    elif key > root.val:
        root.right = deleteNode(root.right, key)
    else:
        if root.left is None:
            return root.right
        elif root.right is None:
            return root.left
        temp = minValueNode(root.right)
        root.val = temp.val
        root.right = deleteNode(root.right, temp.val)
    return root

def minValueNode(node):
    current = node
    while(current.left is not None):
        current = current.left
    return current

# 3. Update
def update(root, old_key, new_key):
    root = deleteNode(root, old_key)
    root = insert(root, new_key)
    return root

# 5. Sort (Inorder Traversal)
def inorder(root):
    if root:
        inorder(root.left)
        print(root.val, end=' ')
        inorder(root.right)
```
