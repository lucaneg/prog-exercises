# Image with gradient, editing an existing one

Open the file you created in exercise `img_gradient.md` and modify the gradient from red to blue to become from yellow (255, 255, 0), to Cyan (0, 255, 255). NOTE: don't create a new file, open the previous one and modify each pixel. You will have to count the bytes in the header and skip them all.

## Solution

```py
from IPython.display import display
from PIL import Image

f = 'gradient.ppm'
header_line_1 = b"P6\n"
header_line_2 = b"256 256\n"header_line_2
header_line_3 = b"255\n"
with open(f, 'r+b') as img:
  # we move after the header
  img.seek(len(header_line_1) + len() + len(header_line_3))
  for i in range(256): # each row
    for j in range(256):
      R = img.read(1) # skip
      img.write(bytes([255]))
      B = img.read(1) # skip

display(Image.open(f))
```
