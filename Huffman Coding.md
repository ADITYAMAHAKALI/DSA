# Huffman Coding

> [!note] Description
> Huffman coding is a popular technique used for lossless data compression. It assigns variable-length codes to input characters, lengths of the assigned codes are based on the frequencies of corresponding characters.

## Intuition
The intuition is efficiency through frequency. In a message, some characters appear way more often than others (like 'e' vs 'z' in English). It makes sense to give short binary codes (like `0` or `11`) to frequent characters and longer codes (like `10101`) to rare characters. This minimizes the total number of bits required to encode the message. We build a binary tree from the bottom up, combining the two least frequent nodes repeatedly, ensuring optimal code length assignment.

## Complexity
- **Time Complexity**: $O(n \log n)$ where $n$ is number of unique characters.
- **Space Complexity**: $O(n)$.

## Implementation (Python)

```python
import heapq

class node:
    def __init__(self, freq, symbol, left=None, right=None):
        self.freq = freq
        self.symbol = symbol
        self.left = left
        self.right = right
        self.huff = ''

    def __lt__(self, nxt):
        return self.freq < nxt.freq

def printNodes(node, val=''):
    newVal = val + str(node.huff)
    if(node.left):
        printNodes(node.left, newVal)
    if(node.right):
        printNodes(node.right, newVal)
    if(not node.left and not node.right):
        print(f"{node.symbol} -> {newVal}")

chars = ['a', 'b', 'c', 'd', 'e', 'f']
freq = [ 5, 9, 12, 13, 16, 45]
nodes = []

for x in range(len(chars)):
    heapq.heappush(nodes, node(freq[x], chars[x]))

while len(nodes) > 1:
    left = heapq.heappop(nodes)
    right = heapq.heappop(nodes)
    left.huff = 0
    right.huff = 1
    newNode = node(left.freq+right.freq, left.symbol+right.symbol, left, right)
    heapq.heappush(nodes, newNode)

printNodes(nodes[0])
```
