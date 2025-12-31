# Hash Table

> [!note] Description
> A Hash Table is a data structure which stores data in an associative manner. In a hash table, data is stored in an array format, where each data value has its own unique index value. Access of data becomes very fast if we know the index of the desired data.

## Operations

### 1. Insert element
- **Complexity**: $O(1)$ on average.

### 2. Delete element
- **Complexity**: $O(1)$ on average.

### 3. Update element
- **Complexity**: $O(1)$ on average.

### 4. Search element
- **Complexity**: $O(1)$ on average.

### 5. Sort the elements
Hash tables are generally unordered. Sorting keys or values requires extracting them.
- **Complexity**: $O(n \log n)$.

## Complexity Summary

| Operation | Time Complexity (Average) | Time Complexity (Worst) |
| :--- | :--- | :--- |
| Access | $O(1)$ | $O(n)$ |
| Search | $O(1)$ | $O(n)$ |
| Insertion | $O(1)$ | $O(n)$ |
| Deletion | $O(1)$ | $O(n)$ |

## Implementation Example (Python)

```python
# Python Dictionaries are Hash Tables
hash_table = {}

# 1. Insert
hash_table['key1'] = 'value1'
hash_table['key2'] = 'value2'

# 3. Update
hash_table['key1'] = 'updated_value'

# 4. Search
if 'key1' in hash_table:
    print(hash_table['key1'])

# 2. Delete
del hash_table['key2']
```
