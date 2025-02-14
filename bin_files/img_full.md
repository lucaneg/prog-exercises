# Image creation

Create a ppm image of 256x256 pixels containing the purple color, which in RGB is given by (127,0,255).

## Solution

```py
from IPython.display import display
from PIL import Image

f = 'purple.ppm'
# either we use the b prefix or we use encode!
header_line_1 = b"P6\n"      # this is fixed
header_line_2 = b"256 256\n" # these are the dimensions of the image
header_line_3 = b"255\n"     # this is fixed in our case
purple = [127,0,255]
with open(f, 'bw') as img:
  img.write(bytes(header_line_1))
  img.write(bytes(header_line_2))
  img.write(bytes(header_line_3))
  for i in range(256):
    for j in range(256):
      img.write(bytes(purple))

display(Image.open(f))
```
