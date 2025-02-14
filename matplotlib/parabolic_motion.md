# Parabolic motion

The equation of the position of a solid that is launched vertically from heigth 0, with a certain initial speed is given by:

$y(t) = v_0 t − \frac{1}{2}gt^2$

where $v_0$ is the initial vertical speed, $g$ is the gravitatonal acceleration on earth surface, and $t$ is time.

Write a Python program that plots the value of $y(t)$ when the $v_0=100\, m/s$ for every second before it touches the ground.

## Solution

```py
import matplotlib.pyplot as plt
import numpy as np

g = 9.8
speed = 100
samples = 100

# if we invert the formula, we can determine the precise moment the object touches the ground
ground_time = (2 * speed) / g

# fast and concise: use numpy
# generate samples values eavenly spaced
times = np.linspace(0, ground_time, samples)
# use vectorization to compute all the hights
heights = speed * times - 0.5 * g * np.square(times)

fig, axes = plt.subplots()
axes.plot(times, heights, 'bo-')
axes.set_xlabel('Time (s)')
axes.set_ylabel('y(t) (m)')
axes.set_title('Parabolic motion')
plt.show()
```
