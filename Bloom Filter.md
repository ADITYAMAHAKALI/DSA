# Bloom Filter

> [!note] Description
> A Bloom filter is a space-efficient probabilistic data structure, conceived by Burton Howard Bloom in 1970, that is used to test whether an element is a member of a set. False positive matches are possible, but false negatives are not.

## Operations

### 1. Insert element
- **Complexity**: $O(k)$ where $k$ is number of hash functions.

### 2. Delete element
Not supported in standard Bloom Filter (Counting Bloom Filter supports it).

### 3. Update element
Not supported.

### 4. Search element (Test Membership)
- **Complexity**: $O(k)$.
- Returns "Possibly in set" or "Definitely not in set".

### 5. Sort the elements
Not applicable.

## Complexity Summary

| Operation | Time Complexity |
| :--- | :--- |
| Insertion | $O(k)$ |
| Search | $O(k)$ |

## Implementation Example (Python)

```python
# Conceptual
class BloomFilter:
    def __init__(self, size, hash_count):
        self.bit_array = [0] * size
        self.size = size
        self.hash_count = hash_count

    # 1. Insert
    def add(self, string):
        for seed in range(self.hash_count):
            result = hash(string + str(seed)) % self.size
            self.bit_array[result] = 1

    # 4. Search (Check membership)
    def check(self, string):
        for seed in range(self.hash_count):
            result = hash(string + str(seed)) % self.size
            if self.bit_array[result] == 0:
                return False
        return True

    # 2. Delete (Not supported in standard Bloom Filter)
    # 3. Update (Not supported)
    # 5. Sort (Not applicable)
```
