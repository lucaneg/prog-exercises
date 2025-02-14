# Image with gradient

Create a 256x256 image that contains an horizontal gradient from red to blue. Column j (from 0 to 255) will have to contain the color (255-j, 0, j).

## Solution

```py
from IPython.display import display
from PIL import Image

f = 'gradient.ppm'
header_line_1 = b"P6\n"
header_line_2 = b"256 256\n"
header_line_3 = b"255\n"
with open(f, 'bw') as img:
  img.write(bytes(header_line_1))
  img.write(bytes(header_line_2))
  img.write(bytes(header_line_3))
  for i in range(256):
    for j in range(256):
      color = [255-j, 0, j]
      img.write(bytes(color))

display(Image.open(f))
```
