# Rabin-Karp Algorithm

> [!note] Description
> The Rabin-Karp algorithm is a string-searching algorithm that uses hashing to find an exact match of a pattern string in a text. It is particularly effective for multiple pattern search.

## Intuition
The intuition is based on rolling hashes. Instead of comparing the pattern character by character at every position (which is slow), we compare the *hash* of the pattern with the *hash* of the current text window. If hashes don't match, the strings definitely don't match. If they do match, we double-check the characters (to avoid hash collisions). Updating the hash for the next window is constant time $O(1)$ due to the rolling hash property, making the algorithm efficient on average.

## Complexity
- **Time Complexity**: $O(N+M)$ average, $O(NM)$ worst case.
- **Space Complexity**: $O(1)$.

## Implementation (Python)

```python
def search(pat, txt, q):
    d = 256
    M = len(pat)
    N = len(txt)
    p = 0
    t = 0
    h = 1

    for i in range(M-1):
        h = (h * d) % q

    for i in range(M):
        p = (d * p + ord(pat[i])) % q
        t = (d * t + ord(txt[i])) % q

    for i in range(N - M + 1):
        if p == t:
            for j in range(M):
                if txt[i + j] != pat[j]:
                    break
            else:
                print("Pattern found at index " + str(i))

        if i < N - M:
            t = (d*(t - ord(txt[i])*h) + ord(txt[i + M])) % q
            if t < 0:
                t = t + q
```
