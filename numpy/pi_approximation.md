# Approximating PI

The following sequences all approximate PI with various degrees of precision:

![seq](https://raw.githubusercontent.com/lucaneg/inf1mod2/master/pi.png)

For each sequence (where `x` is the name of the sequence - one of `a`, `b`, `c`, `d`, `e`), write:

- a function `element_x` that takes as a parameter a numpy array containing the values of from the start of the sequence (either 1 or 0) to a given `n`, and returns the value for `x_n` (for instance, to compute `a_4` you would call `sequence_a(np.array([1, 2, 3, 4]))`)

- a function `sequence_x` that takes as a parameter a value for `n` and computes the elements `x_1, ..., x_n` (or `x_0, ..., x_n`, depending on the sequence), returning them as a numpy array. Such a function must construct, for each element, the array containing the values of `k` and use `element_x` to compute the corresponding element
- a function `stats` that takes as a parameter a sequence `x_1, ..., x_n` (or `x_0, ..., x_n`, depending on the sequence) and prints the following information:
  - the length of the sequence, by passing the appropriate parameter to `print_sequence_length`
  - the last element of the sequence, by passing the appropriate parameter to `print_last_element`
  - the absolute distance between PI and the last element, `|math.pi - x_n|`, by passing the appropriate parameter to `print_error`
  - the trend of the sequence (increasing, decreasing, or oscillating) by passing a value (positive, negative, or zero) to `print_trend`. The value to pass must be determined by computing the ratio of each element in the sequence and the previous one (`x_i / x_(i-1)`), and checking if all of the ratios are positive, negative, or alternating between positive and negative

## Template

```py
import numpy as np
def print_sequence_length(length):
  print('number of elements:', length)

def print_last_element(last):
  print('last element:', f'{last:<.4f}')

def print_error(error):
  print('approximation error:', f'{error:<.4f}')

def print_trend(trend):
  if trend < 0:
    print('decreasing')
  elif trend > 0:
    print('increasing')
  else:
    print('oscillating')

if __name__ == '__main__':
  import sys
  n = int(input())

  print('number of elements:', n)

  print('sequence a')
  stats(sequence_a(n))

  print('sequence b')
  stats(sequence_b(n))

  print('sequence c')
  stats(sequence_c(n))

  print('sequence d')
  stats(sequence_d(n))

  print('sequence e')
  stats(sequence_e(n))
```

## Testcases


### 1

IN:
```
3
```

OUT:
```
number of elements: 3
sequence a
number of elements: 3
last element: 3.4667
approximation error: 0.3251
increasing
sequence b
number of elements: 3
last element: 2.8577
approximation error: 0.2839
decreasing
sequence c
number of elements: 3
last element: 3.1362
approximation error: 0.0054
decreasing
sequence d
number of elements: 3
last element: 3.1379
approximation error: 0.0037
decreasing
sequence e
number of elements: 3
last element: 3.1416
approximation error: 0.0000
decreasing
```

### 2

IN:
```
5
```

OUT:
```
number of elements: 5
sequence a
number of elements: 5
last element: 3.3397
approximation error: 0.1981
oscillating
sequence b
number of elements: 5
last element: 2.9634
approximation error: 0.1782
decreasing
sequence c
number of elements: 5
last element: 3.1402
approximation error: 0.0014
decreasing
sequence d
number of elements: 5
last element: 3.1413
approximation error: 0.0003
oscillating
sequence e
number of elements: 5
last element: 3.1416
approximation error: 0.0000
oscillating
```

### 2

IN:
```
100
```

OUT:
```
number of elements: 100
sequence a
number of elements: 100
last element: 3.1316
approximation error: 0.0100
oscillating
sequence b
number of elements: 100
last element: 3.1321
approximation error: 0.0095
decreasing
sequence c
number of elements: 100
last element: 3.1416
approximation error: 0.0000
decreasing
sequence d
number of elements: 100
last element: 3.1416
approximation error: 0.0000
oscillating
sequence e
number of elements: 100
last element: 3.1416
approximation error: 0.0000
oscillating
```

### 3

IN:
```
15
```

OUT:
```
number of elements: 15
sequence a
number of elements: 15
last element: 3.2082
approximation error: 0.0666
oscillating
sequence b
number of elements: 15
last element: 3.0794
approximation error: 0.0622
decreasing
sequence c
number of elements: 15
last element: 3.1415
approximation error: 0.0001
decreasing
sequence d
number of elements: 15
last element: 3.1416
approximation error: 0.0000
oscillating
sequence e
number of elements: 15
last element: 3.1416
approximation error: 0.0000
oscillating
```

### 4

IN:
```
1000
```

OUT:
```
number of elements: 1000
sequence a
number of elements: 1000
last element: 3.1406
approximation error: 0.0010
oscillating
sequence b
number of elements: 1000
last element: 3.1406
approximation error: 0.0010
decreasing
sequence c
number of elements: 1000
last element: 3.1416
approximation error: 0.0000
decreasing
sequence d
number of elements: 1000
last element: 3.1416
approximation error: 0.0000
oscillating
sequence e
number of elements: 1000
last element: 3.1416
approximation error: 0.0000
oscillating
```

## Solution

```py
import math
import sys

def element_a(k):
  return 4 * np.sum(np.power(-1, k + 1) / (2 * k - 1))

def element_b(k):
  return math.sqrt((6 * np.sum(np.power(k, -2))))

def element_c(k):
  return math.pow((90 * np.sum(np.power(k, -4))), 1/4)

def element_d(k):
  return (6/math.sqrt(3)) * np.sum(np.power(-1, k) / (np.power(3, k) * (2 * k + 1)))

def element_e(k):
  return 16 * np.sum(np.power(-1, k) / (np.power(5, 2 * k + 1) * (2 * k + 1))) - 4 * np.sum(np.power(-1, k) / (np.power(239, 2 * k + 1) * (2 * k + 1)))

def sequence_a(n):
  elements = np.empty(n)
  for pos in range(n):
    k_from_1 = np.arange(1, pos + 2, dtype=np.float128)
    elements[pos] = element_a(k_from_1)
  return elements

def sequence_b(n):
  elements = np.empty(n)
  for pos in range(n):
    k_from_1 = np.arange(1, pos + 2, dtype=np.float128)
    elements[pos] = element_b(k_from_1)
  return elements

def sequence_c(n):
  elements = np.empty(n)
  for pos in range(n):
    k_from_1 = np.arange(1, pos + 2, dtype=np.float128)
    elements[pos] = element_c(k_from_1)
  return elements

def sequence_d(n):
  elements = np.empty(n)
  for pos in range(n):
    k_from_0 = np.arange(0, pos + 2, dtype=np.float128)
    elements[pos] = element_d(k_from_0)
  return elements

def sequence_e(n):
  elements = np.empty(n)
  for pos in range(n):
    k_from_0 = np.arange(0, pos + 2, dtype=np.float128)
    elements[pos] = element_e(k_from_0)
  return elements

def stats(sequence):
  print_sequence_length(sequence.size)
  print_last_element(sequence[-1])
  print_error(math.fabs(math.pi - sequence[-1]))
  ratios = sequence[1:] / sequence[:-1]
  if np.array_equal(ratios[:-1][ratios[:-1] > ratios[1:]], ratios[:-1]):
    print_trend(-1)
  elif np.array_equal(ratios[:-1][ratios[:-1] < ratios[1:]], ratios[:-1]):
    print_trend(1)
  else:
    print_trend(0)
```
