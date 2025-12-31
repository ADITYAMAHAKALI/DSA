# Skip List

> [!note] Description
> A skip list is a probabilistic data structure that allows $O(\log n)$ search complexity as well as $O(\log n)$ insertion complexity within an ordered sequence of elements. Thus it can get the best features of a sorted array (for searching) and a linked list (for insertion).

## Operations

### 1. Insert element
- **Complexity**: $O(\log n)$ average.

### 2. Delete element
- **Complexity**: $O(\log n)$ average.

### 3. Update element
- **Complexity**: $O(\log n)$ average.

### 4. Search element
- **Complexity**: $O(\log n)$ average.

### 5. Sort the elements
Skip list maintains elements in sorted order.
- **Complexity**: $O(n)$ to traverse.

## Complexity Summary

| Operation | Time Complexity (Average) | Time Complexity (Worst) |
| :--- | :--- | :--- |
| Access | $O(\log n)$ | $O(n)$ |
| Search | $O(\log n)$ | $O(n)$ |
| Insertion | $O(\log n)$ | $O(n)$ |
| Deletion | $O(\log n)$ | $O(n)$ |

## Implementation Example (Python)

```python
import random

class Node:
    def __init__(self, key, level):
        self.key = key
        self.forward = [None] * (level + 1)

class SkipList:
    def __init__(self, max_lvl, P):
        self.MAX_LVL = max_lvl
        self.P = P
        self.header = Node(-1, self.MAX_LVL)
        self.level = 0

    def randomLevel(self):
        lvl = 0
        while random.random() < self.P and lvl < self.MAX_LVL:
            lvl += 1
        return lvl

    # 1. Insert
    def insertElement(self, key):
        update = [None] * (self.MAX_LVL + 1)
        current = self.header
        for i in range(self.level, -1, -1):
            while current.forward[i] and current.forward[i].key < key:
                current = current.forward[i]
            update[i] = current
        current = current.forward[0]
        if current is None or current.key != key:
            rlevel = self.randomLevel()
            if rlevel > self.level:
                for i in range(self.level + 1, rlevel + 1):
                    update[i] = self.header
                self.level = rlevel
            n = Node(key, rlevel)
            for i in range(rlevel + 1):
                n.forward[i] = update[i].forward[i]
                update[i].forward[i] = n

    # 4. Search
    def searchElement(self, key):
        current = self.header
        for i in range(self.level, -1, -1):
            while current.forward[i] and current.forward[i].key < key:
                current = current.forward[i]
        current = current.forward[0]
        if current and current.key == key:
            print("Found key:", key)
        else:
            print("Key not found:", key)

    # 2. Delete (Simplified)
    def deleteElement(self, key):
        update = [None] * (self.MAX_LVL + 1)
        current = self.header
        for i in range(self.level, -1, -1):
            while current.forward[i] and current.forward[i].key < key:
                current = current.forward[i]
            update[i] = current
        current = current.forward[0]
        if current and current.key == key:
            for i in range(self.level + 1):
                if update[i].forward[i] != current:
                    break
                update[i].forward[i] = current.forward[i]
            while self.level > 0 and self.header.forward[self.level] is None:
                self.level -= 1

    # 3. Update (Delete and Insert)
    def updateElement(self, old_key, new_key):
        self.deleteElement(old_key)
        self.insertElement(new_key)

    # 5. Sort (Traverse level 0)
    def displayList(self):
        head = self.header.forward[0]
        while head:
            print(head.key, end=" ")
            head = head.forward[0]
        print("")
```
