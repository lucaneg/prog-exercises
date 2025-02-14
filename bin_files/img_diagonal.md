# Image with diagonal line

Create a 256x256 purple image (127,0,255) with a diagonal orange line (255, 165, 0).

## Solution

```py
from IPython.display import display
from PIL import Image

f = 'purple-stripe.ppm'
header_line_1 = b"P6\n"
header_line_2 = b"256 256\n"
header_line_3 = b"255\n"
purple = [127,0,255]
orange = [255,165,0]
with open(f, 'bw') as img:
  img.write(bytes(header_line_1))
  img.write(bytes(header_line_2))
  img.write(bytes(header_line_3))
  for i in range(256):
    for j in range(256):
      # we just need to change the color on the diagonal, that is, where i == j
      if i == j:
        img.write(bytes(orange))
      else:
        img.write(bytes(purple))

display(Image.open(f))
```
