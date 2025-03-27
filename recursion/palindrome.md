# Palindrome matching

Write a recursive function `is_palindrome(target)` that checks if a given string is palindrome.

Hint: you can check the first and last character of the string, and then recursively inspect the remainder of the string until it is empty.

## Template

```py
if __name__ == '__main__':
    print(is_palindrome(input()))
```

## Testcases

### 1

IN:
```
abc
```

OUT:
```
False
```

### 2

IN:
```
abcba
```

OUT:
```
True
```

### 3

IN:
```
itopinonaevanonipoti
```

OUT:
```
True
```

### 4

IN:
```
questa stringa non e' palindroma
```

OUT:
```
False
```

## Solution

```py
def is_palindrome(target):
    if len(target) < 2:
        return True
    if target[0] != target[-1]:
        return False
    return is_palindrome(target[1:-1])
```
