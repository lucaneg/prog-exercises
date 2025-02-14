# Sound file containing the A note

Write a 5 second WAV file with the A note (LA), that is, a signal that oscillates 440 times per second. Remember that the values you write to the file must be integers between 0 and 256: find a way to scale it.

## Solution

```py
import random
import math
import wave
import IPython

fname = 'note.wav'
samplerate = 44100
length = 5
amplitude = 24

def make_note():
  note = []
  freq = 440
  for sample in range(0, length * samplerate):
    sample_time = sample / samplerate
    sample = amplitude * math.sin(2 * math.pi * freq * sample_time)

    # since we have to convert these to bytes, we must start from an
    # integer value between 0 and 255. Samples here will range between
    # -amplitude and +amplitude, so we have to shift the value up
    # in this way, we have values between 0 and 2amplitude
    sample = sample + amplitude
    note.append(int(sample))
  return note

with wave.open(fname, 'wb') as f:
  f.setnchannels(1)
  f.setsampwidth(1)
  f.setframerate(samplerate)
  samples = make_note()
  f.writeframes(bytes(samples))

IPython.display.Audio(fname, autoplay=True)
```
