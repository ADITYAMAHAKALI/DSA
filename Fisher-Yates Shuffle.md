# Fisher-Yates Shuffle

> [!note] Description
> The Fisher-Yates shuffle is an algorithm for generating a random permutation of a finite sequence—in plain terms, the algorithm shuffles the sequence.

## Complexity
- **Time Complexity**: $O(n)$.
- **Space Complexity**: $O(1)$.

## Implementation (Python)

```python
import random

def fisher_yates_shuffle(arr):
    n = len(arr)
    for i in range(n-1, 0, -1):
        j = random.randint(0, i)
        arr[i], arr[j] = arr[j], arr[i]
    return arr
```
