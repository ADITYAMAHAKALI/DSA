# Number Theory Basics

Problems revolving around digits, bases, and modular arithmetic.

## Problems

### 1. Greatest Common Divisor
**Intuition**: Euclidean Algorithm.

```python
def gcd(a, b):
    while b:
        a, b = b, a % b
    return a
```

### 2. Excel Sheet Column Title
**Problem**: 1 -> A, 2 -> B, ... 26 -> Z, 27 -> AA.
**Intuition**: Base-26 conversion but 1-indexed. Use `(n - 1) % 26` to map to 0-25.

```python
def convert_to_title(n):
    res = []
    while n:
        n -= 1
        res.append(chr(n % 26 + ord('A')))
        n //= 26
    return "".join(res[::-1])
```

### 3. Palindrome Number
**Problem**: Check if integer `x` is a palindrome.
**Intuition**: Revert half of the number to avoid overflow. Or revert full number.

```python
def is_palindrome(x):
    if x < 0: return False
    original, rev = x, 0
    while x:
        rev = rev * 10 + x % 10
        x //= 10
    return original == rev
```

### 4. Happy Number
**Problem**: Sum of squares of digits repeatedly. If it reaches 1, it's happy. If it loops, it's not.
**Intuition**: Use Floyd's Cycle Detection (slow/fast pointers) or a Set.

```python
def is_happy(n):
    def get_next(number):
        total_sum = 0
        while number > 0:
            digit = number % 10
            number //= 10
            total_sum += digit ** 2
        return total_sum

    slow, fast = n, get_next(n)
    while fast != 1 and slow != fast:
        slow = get_next(slow)
        fast = get_next(get_next(fast))

    return fast == 1
```

### 5. Plus One
**Problem**: Add one to an integer represented as an array of digits.
**Intuition**: Process from end. Handle carry.

```python
def plus_one(digits):
    for i in range(len(digits) - 1, -1, -1):
        if digits[i] == 9:
            digits[i] = 0
        else:
            digits[i] += 1
            return digits
    return [1] + digits
```
