# Linked List

> [!note] Description
> A linked list is a linear data structure, in which the elements are not stored at contiguous memory locations. The elements in a linked list are linked using pointers.

## Operations

### 1. Insert element
- **At Beginning**: $O(1)$.
- **At End**: $O(n)$ (or $O(1)$ with tail pointer).
- **At Index**: $O(n)$ to traverse to the position.

### 2. Delete element
- **At Beginning**: $O(1)$.
- **At End**: $O(n)$ (need to find second to last).
- **At Index**: $O(n)$.

### 3. Update element
- **Complexity**: $O(n)$ (must traverse to find the node).

### 4. Search element
- **Complexity**: $O(n)$ (must traverse sequentially).

### 5. Sort the elements
- **Complexity**: $O(n \log n)$ (e.g., Merge Sort for Linked Lists).

## Complexity Summary

| Operation | Time Complexity |
| :--- | :--- |
| Access | $O(n)$ |
| Search | $O(n)$ |
| Insertion | $O(1)$ (at head) |
| Deletion | $O(1)$ (at head) |

## Implementation Example (Python)

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None

    # 1. Insert (at beginning)
    def push(self, new_data):
        new_node = Node(new_data)
        new_node.next = self.head
        self.head = new_node

    # 2. Delete (by key)
    def delete_node(self, key):
        temp = self.head
        if (temp is not None):
            if (temp.data == key):
                self.head = temp.next
                temp = None
                return
        while(temp is not None):
            if temp.data == key:
                break
            prev = temp
            temp = temp.next
        if(temp == None):
            return
        prev.next = temp.next
        temp = None
```
