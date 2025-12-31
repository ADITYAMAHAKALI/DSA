# Stack

> [!note] Description
> A stack is a linear data structure which follows a particular order in which the operations are performed. The order may be LIFO (Last In First Out) or FILO (First In Last Out).

## Operations

### 1. Insert element (Push)
Adds an item to the stack. If the stack is full, then it is said to be an Overflow condition.
- **Complexity**: $O(1)$.

### 2. Delete element (Pop)
Removes an item from the stack. The items are popped in the reversed order in which they are pushed. If the stack is empty, then it is said to be an Underflow condition.
- **Complexity**: $O(1)$.

### 3. Update element
Generally not a standard operation for a pure stack (access is limited to top). If implemented as array/list, $O(n)$ to find and update arbitrarily, or $O(1)$ to update top.

### 4. Search element
- **Complexity**: $O(n)$ (scan through the stack).

### 5. Sort the elements
- **Complexity**: $O(n^2)$ using only stack operations (or $O(n \log n)$ extracting to array).

## Complexity Summary

| Operation | Time Complexity |
| :--- | :--- |
| Access | $O(n)$ (worst case) |
| Search | $O(n)$ |
| Insertion (Push) | $O(1)$ |
| Deletion (Pop) | $O(1)$ |

## Implementation Example (Python)

```python
stack = []

# 1. Insert (Push)
stack.append('a')
stack.append('b')

# 2. Delete (Pop)
print(stack.pop()) # 'b'

# Peek (View Top)
if stack:
    print(stack[-1])
```
