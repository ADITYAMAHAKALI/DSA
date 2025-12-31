# Knuth-Morris-Pratt (KMP) Algorithm

> [!note] Description
> The Knuth-Morris-Pratt string-searching algorithm searches for occurrences of a "word" $W$ within a main "text string" $T$ by employing the observation that when a mismatch occurs, the word itself embodies sufficient information to determine where the next match could begin, thus bypassing re-examination of previously matched characters.

## Intuition
The intuition is to never move backward in the main text. When we match characters but then hit a mismatch, we don't want to start all over from the next character in the text (which would be slow). Instead, we use a pre-calculated "LPS" (Longest Prefix Suffix) array to know exactly how much of the pattern we've effectively already matched. This allows us to slide the pattern forward intelligently, skipping redundant comparisons.

## Complexity
- **Time Complexity**: $O(N + M)$ where $N$ is text length, $M$ is pattern length.
- **Space Complexity**: $O(M)$.

## Implementation (Python)

```python
def compute_lps_array(pat, M, lps):
    len = 0
    lps[0] = 0
    i = 1
    while i < M:
        if pat[i] == pat[len]:
            len += 1
            lps[i] = len
            i += 1
        else:
            if len != 0:
                len = lps[len-1]
            else:
                lps[i] = 0
                i += 1

def KMPSearch(pat, txt):
    M = len(pat)
    N = len(txt)
    lps = [0]*M
    compute_lps_array(pat, M, lps)

    i = 0
    j = 0
    while i < N:
        if pat[j] == txt[i]:
            i += 1
            j += 1
        if j == M:
            print("Found pattern at index " + str(i-j))
            j = lps[j-1]
        elif i < N and pat[j] != txt[i]:
            if j != 0:
                j = lps[j-1]
            else:
                i += 1
```
