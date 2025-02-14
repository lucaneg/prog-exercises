# Sound file containing noise

Write a 5 second WAV file with the noise, that is, a signal containing random samples.

## Solution

```py
import random
import math
import wave
import IPython

fname = 'noise.wav'
samplerate = 44100
length = 5
amplitude = 24

def make_noise():
  noise = []
  for i in range(samplerate * length):
    # since we generate noise, we don't need to use the wave formula above,
    # we'd need to use it if we want to produce actual notes
    noise.append(random.randint(0, amplitude))
  return noise

def make_horn():
  noise = []
  for i in range(samplerate * length):
    # since we generate noise, we don't need to use the wave formula above,
    # we'd need to use it if we want to produce actual notes
    noise.append(i % (5 * amplitude))
  return noise

with wave.open(fname, 'wb') as f:
  f.setnchannels(1)
  f.setsampwidth(1)
  f.setframerate(samplerate)
  samples = make_horn()
  f.writeframes(bytes(samples))

IPython.display.Audio(fname, autoplay=True)
```
