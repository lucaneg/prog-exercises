# Sound wave

# Example: Sound wave part 2

Recall example 2 from the binary files lecture. A sound wave is identified by the formula $s(t) = A \sin(2 \pi f t)$, where $A$ is the amplitude and $f$ is the frequency. Since computers are discrete systems, the formula to use when modeling such a wave becomes $s_n = A \sin(2\pi f \frac{n}{r}), n = 0, 1, \dots, m*r$ for a sound that lasts $m$ seconds and is sampled $r$ times each second. To generate a sound wave for the `A` note (`LA`) we had to generate a wave in a for loop similar to this:
```python
for sample in range(0, length * samplerate):
  sample_time = sample / samplerate
  sample = amplitude * math.sin(2 * math.pi * freq * sample_time)
  sample = sample + amplitude
  note.append(int(sample))
```
Generating the same wave avoids the for loop, and is much faster:

## Solution

```py
import math
import numpy as np

samplerate = 44100
length = 5
amplitude = 24
freq = 440

time_intervals = np.linspace(0, length, samplerate * length)
note_numpy = amplitude + amplitude * np.sin(2 * math.pi * freq * time_intervals)
print('numpy version:')
print(note_numpy.size)
print(note_numpy[:10])
```
