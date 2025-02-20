# Exponential Decay

The exponential decay is a function describing how a quantity can decay over time. Specifically, it describes the decay whenever the rate at which the quantity decreases depends on its current value. The decay is given by the formula:

$y(t) = y_0 \cdot e^{-kt}$

where $y(t)$ is the quantity at a given time, $y_0$ is the quantity at the start of the decay process, and $k$ is the rate of decrease.

## Solution

```py
import numpy as np
import matplotlib.pyplot as plt

y0 = 100
k = 0.85
t = np.linspace(0, 10, 100)
y = y0 * np.exp(-k * t)

plt.plot(t, y)
plt.title("Exponential Decay")
plt.xlabel("Time")
plt.ylabel("Quantity")
plt.grid()
```
