# Palindrome matching

Write a function `is_palindrome(target)` that checks if a given string is palindrome using stacks and queues.

Hint: you can place all characters in both a stack and a queue, and when retrieving elements from them they must always match.

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
i topi non avevano nipoti
```

OUT:
```
False
```

### 4

IN:
```
itopinonavevanonipoti
```

OUT:
```
True
```

## Solution

```py
def is_palindrome(target):
  queue = [c for c in target]
  stack = [c for c in target]

  while queue and stack:
    popped_from_queue = queue.pop(0)
    popped_from_stack = stack.pop()
    if popped_from_queue != popped_from_stack:
      return False
  return True
```
