# Cumulative Operations

Generate a random 1D integer array of length 20, with values between 1 (inclusive) and 10 (exclusive). Calculate the cumulative sum, cumulative product, and cumulative maximum of the array. Each of these are 1D arrays of the same length as the original one, having in each cell the partial result of the operation. For instance, the cumulative sum of a = [1, 2, 3] is r = [1, 3, 6], that is, [a[0], a[0] + a[1], a[0] + a[1] + a[2]].

## Solution

```py
import numpy as np

rng = np.random.default_rng()
target = rng.integers(1, 10, size=20)
print('target: ', target)

cumulative_sum = np.zeros_like(target)
cumulative_prod = np.zeros_like(target)
cumulative_max = np.zeros_like(target)

for i in range(len(target)):
  cumulative_sum[i] = target[:i+1].sum()
  cumulative_max[i] = target[:i+1].max()
  cumulative_prod[i] = target[:i+1].prod()

print('cumulative_sum: ', cumulative_sum)
print('cumulative_prod: ', cumulative_prod)
print('cumulative_max: ', cumulative_max)
```
