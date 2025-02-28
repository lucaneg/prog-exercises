# Motion

Write a class `Motion` that models a uniformly accelerated motion, given an initial velocity and an acceleration. The class should have two attributes, both initialized in a constructor:
- `v0` holding the initial velocity
- `a` holding the acceleration

The class should then have two methods:
- `calculate_x` that takes a time `t` as parameter, and returns the displacement `v_0 * t + (1/2) * a * t**2`
- `calculate_v` that takes a time `t` as parameter, and returns the velocity `v_0 + a * t`

## Template

```py
if __name__ == '__main__':
    f = Motion(float(input()), float(input()))
    print(f.calculate_x(float(input())))
    print(f.calculate_v(float(input())))
```

## Testcases

### 1

IN:
```
3
1
10
10
```

OUT:
```
80.0
13.0
```

### 2

IN:
```
1.2
4.5
20
20
```

OUT:
```
924.0
91.2
```

## Solution

```py
class Motion:
  def __init__(self, v0, a):
    self.v0 = v0
    self.a = a

  def calculate_x(self, t):
    return self.v0*t + 0.5*self.a*(t**2)

  def calculate_v(self, t):
    return self.v0 + self.a*t
```
