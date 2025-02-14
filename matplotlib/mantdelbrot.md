# The Mandelbrot set

A fractal is a geometric structure that can be broken down into smaller and smaller parts, with each one maintaining the same shape of the original structure. You can think of a fractal like an image with an infinite amount of pixels, where you can zoom in indefinitely without losing definition while still seeing the same patterns.

![fractal1](https://georgemdallas.files.wordpress.com/2014/05/von_koch_curve-1.jpg)
![fractal2](https://www.stsci.edu/~lbradley/seminar/images/sierpinski.gif)

One of the most famous fractals is the Mandelbrot set, that is defined using complex numbers. The fractal is defined through the sequence $z_{0} = 0, z_{n+1} = z_{n}^2 + c$ for some complex constant $c$. The Mandelbrot set contains all of those constants $c$ such that the sequence remains bounded, that is, if $\lim_{n\to\infty} z_n < k \in \mathbb{N}$. For instance, if $c = 1$, the sequence is $0, 1, 2, 5, 26, \dots$ and is not finite, so $1$ is not part of the Mandelbrot set. Instead, if $c = -1$, the sequence is $0, -1, 0, -1, \dots$ and it is finite, thus $-1$ is in the Mandelbrot set. It has been proven that the sequence cannot be bounded if $|z_n| > 2$: since $z_0 = c$, the Mandelbrot set only contains elements of the complex plain within a radius of 2 from the origin.

We will now build an approximation of the Mandelbrot set (the set is infinite, so we cannot compute it fully) and display it with matplotlib.

## Solution

```py
import numpy as np
import matplotlib.pyplot as plt

# we limit our search to the square that surely contains the full set
lbound, ubound = -0.7, 0.0

# we sample the set by using 1000000 complex values for c
# composed by 1000 possible real parts and 1000 possible imaginary parts
samples_per_axis = 500

# for each value of c, we determine for how many iterations the sequence
# remains bounded
max_iter = 100

# computation of the number of iterations for a given c using the sequence
def mandelbrot(c):
  z = 0
  iter = 0
  while abs(z) <= 2 and iter < max_iter:
    z = z*z + c
    iter += 1
  return iter

# we define the components that we will use for real and imaginary parts
samples = np.linspace(lbound, ubound, samples_per_axis)

# we compute the grid: for each sample, we build the complex value
# sample[j] + j*sample[i] (i and j are swapped to follow the order
# in which the pixels will be drawn)
mandelbrot_grid = np.zeros((samples_per_axis, samples_per_axis))
for i in range(samples_per_axis):
  for j in range(samples_per_axis):
    mandelbrot_grid[i, j] = mandelbrot(samples[j] + 1j*samples[i])

fig, ax = plt.subplots(figsize=(10,10))
# imshow uses values from an array to display individual pixels
img = ax.imshow(mandelbrot_grid, extent=(lbound, ubound, lbound, ubound), cmap='hot')
ax.set_title('Mandelbrot Set')
ax.set_xlabel('Real')
ax.set_ylabel('Imaginary')
fig.colorbar(img, ax=ax, label='Iterations')
```
