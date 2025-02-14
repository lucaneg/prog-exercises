# Harmonic oscillator

![oscillator.gif](https://upload.wikimedia.org/wikipedia/commons/0/07/Easy_harmonic_oscillator.gif)

An Harmonic oscillator (as in the pic from wikipedia) respects the following equations:

1. Position of the orange square: $x(t) = A cos(\omega t + \phi)$
2. Speed of the orange square: $v(t) = x'(t) = -A\omega sin(\omega t + \phi)$
3. Kinetic energy of the orange square: $K(t) = \frac{1}{2} m (v(t))^2 = \frac{1}{2} m (x'(t))^2 = \frac{m}{2}(-A\omega sin(\omega t + \phi))^2$

Let $A=1, \omega=\pi, \phi=0, m=1$ and let the oscillator run for half a period (t=0, t=1). Sample 1000 values of $x$, $|v|$, and $K$ and plot them **in the same graph**.

## Solution

```py
import math
import numpy as np
import matplotlib.pyplot as plt

omega = math.pi
A = 1
phi = 0
m = 1
min_t = 0
max_t = 1
samples = 1000

# easiest way: numpy and vectorization!
time_samples = np.linspace(min_t, max_t, samples)
pos_samples = A * np.cos(omega * time_samples + phi)
speed_samples = np.abs(-A * omega * np.sin(omega * time_samples + phi))
energy_samples = 0.5 * m * np.square(-A * omega * np.sin(omega * time_samples + phi))

fig, axes = plt.subplots()
axes.set_title('Harmonic motion')
axes.set_xlabel('Time (s)')
# multiple plots on a single axes
axes.plot(time_samples, pos_samples, '-', label='position')
axes.plot(time_samples, speed_samples, '-', label='speed')
axes.plot(time_samples, energy_samples, '-', label='energy')
axes.legend()
```
