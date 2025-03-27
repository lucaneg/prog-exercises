# Collatz conjecture

The Collatz conjecture is one of the most famous unsolved problems in mathematics. The conjecture defines the following function:

![transformer](https://wikimedia.org/api/rest_v1/media/math/render/svg/9b2a03faf9d31a8de0abb3c4a3d318490105da12)

and then repeatedly applies it to a number, until it reaches 1:

![series](https://wikimedia.org/api/rest_v1/media/math/render/svg/0f220f8b8d9aaa456552e64310e8fbe65e356718)

`k` is called the *stopping time* of the sequence, meaning the number of steps `f` must be applied for the sequence to reach 1. The conjecture states that the stopping time is always finite regardless of the starting `n`, meaning that the sequence of elements `a_i` always reaches 1. No general proof has currently been found.

Write a function `f(n)` that implements the Collatz transformer `f(n)`. Then, write a **recursive** function `stopping_time(n)` that calculates the number `k` where the sequence reaches 1:
- if `n==1`, the stopping time is 0
- otherwise, the stopping time can be found by adding 1 to the stopping time of the transformed number

## Template

```py
if __name__ == '__main__':
  n = int(input())
  print(stopping_time(n))
```

## Testcases

### 1

IN:
```
1
```

OUT:
```
0
```

### 2

IN:
```
97
```

OUT:
```
118
```

### 3

IN:
```
43
```

OUT:
```
29
```

### 4

IN:
```
3
```

OUT:
```
7
```

### 5

IN:
```
2
```

OUT:
```
1
```

### 6

IN:
```
10
```

OUT:
```
6
```

### 7

IN:
```
9
```

OUT:
```
19
```

## Solution

```py
def f(n):
  if n % 2 == 0:
    return n // 2
  else:
    return (3 * n) + 1

def stopping_time(n):
  if n == 1:
    return 0
  else:
    return 1 + stopping_time(f(n))
```
