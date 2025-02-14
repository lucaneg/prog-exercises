# Statistical Operations

Generate a random NumPy array with 100 elements and calculate its mean and standard deviation. Then, compare the values you computed with the ones returned by array.mean() and array.std().

## Solution

```py
import numpy as np

rng = np.random.default_rng()
target = rng.random(100)
print('target: ', target)

mean = target.sum() / target.size
np_mean = target.mean()
print('mean:', mean, np_mean, mean == np_mean)

# mean is getting broadcasted!
std = np.sqrt(np.sum(np.square(target - mean)) / 100)
np_std = target.std()
print('std:', std, np_std, std == np_std)
```
