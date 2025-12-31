# Trie

> [!note] Description
> Trie is an efficient information reTrieval data structure. Using Trie, search complexities can be brought to optimal limit (key length). If we store keys in binary search tree, a well balanced BST will need time proportional to $M \times \log N$, where $M$ is maximum string length and $N$ is number of keys in tree. Using Trie, we can search the key in $O(M)$ time.

## Operations

### 1. Insert element
- **Complexity**: $O(M)$ where $M$ is the length of the key.

### 2. Delete element
- **Complexity**: $O(M)$.

### 3. Update element
Delete old, insert new.
- **Complexity**: $O(M)$.

### 4. Search element
- **Complexity**: $O(M)$.

### 5. Sort the elements
Pre-order traversal of Trie gives sorted strings (lexicographically).
- **Complexity**: $O(N \times M)$ where $N$ is total number of keys.

## Complexity Summary

| Operation | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| Search | $O(M)$ | $O(N \times M)$ |
| Insertion | $O(M)$ | $O(N \times M)$ |
| Deletion | $O(M)$ | $O(N \times M)$ |

## Implementation Example (Python)

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False

class Trie:
    def __init__(self):
        self.root = TrieNode()

    # 1. Insert
    def insert(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end_of_word = True

    # 4. Search
    def search(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
        return node.is_end_of_word

    # 2. Delete
    def delete(self, word):
        def _delete(node, word, depth):
            if depth == len(word):
                if not node.is_end_of_word:
                    return False
                node.is_end_of_word = False
                return len(node.children) == 0
            char = word[depth]
            if char not in node.children:
                return False
            should_delete_child = _delete(node.children[char], word, depth + 1)
            if should_delete_child:
                del node.children[char]
                return len(node.children) == 0
            return False
        _delete(self.root, word, 0)

    # 3. Update
    def update(self, old_word, new_word):
        self.delete(old_word)
        self.insert(new_word)

    # 5. Sort (lexicographical traversal)
    def sort_elements(self):
        result = []
        def _dfs(node, prefix):
            if node.is_end_of_word:
                result.append(prefix)
            for char in sorted(node.children.keys()):
                _dfs(node.children[char], prefix + char)
        _dfs(self.root, "")
        return result
```
