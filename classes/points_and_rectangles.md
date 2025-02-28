# Points and rectangles

A point in a Cartesian plane can be represented by its x and y coordinates. A rectangle can insted be represented by the coordinates of its lower-left corner, together with its length, width, and rotation angle. Let's write a `Point` class and a `Rectangle` class modeling these two concepts.

## Solution

```py
class Point:
  def __init__(self, x, y):
    self.x = x
    self.y = y

class Rectangle:
  def __init__(self, corner, width, height, angle):
    self.corner = corner
    self.width = width
    self.height = height
    self.angle = angle

  def print_rect(self):
    print('rectangle with bottom-left corner at (', self.corner.x, self.corner.y,
          ') with width', self.width, 'and height', self.height, 'and angle', self.angle)

p = Point(3, 7)
p2 = Point(10, 0)

print(p.x)
print(p2.x)
r = Rectangle(p, 5, 2, 15)
print(r.width)
r.print_rect()
```
