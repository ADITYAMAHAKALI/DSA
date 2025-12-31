# Fisher-Yates Shuffle

> [!note] Description
> The Fisher-Yates shuffle is an algorithm for generating a random permutation of a finite sequence—in plain terms, the algorithm shuffles the sequence.

## Intuition
The intuition is like picking names out of a hat. You write all names on slips of paper and put them in a hat. You draw one name at random and set it aside as the last element of your new shuffled list. Then you draw another from the remaining slips for the second-to-last spot, and so on. In the array implementation, we swap the "drawn" element to the end of the unvisited portion of the array, effectively shrinking the "hat" by one each time.

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
