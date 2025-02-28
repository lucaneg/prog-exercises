# Intervals with arithmetics

Let's create an `Interval` class modeling the closed interval `[low, high]` representing all integer values `x` such that `low <= x <= high`, and let's add operators for supporting the usage of `+`, `-`, `*`, `/`, `>`, `>=`, `<`, `<=`, `==`, `!=`, `+=`, `-=`, `*=`, and `/=`. The definition of the rules for interval arithmetics can be found [here](https://en.wikipedia.org/wiki/Interval_arithmetic#Interval_operators).

All of the operators of the Interval class should support also integer arguments, interpreting them as singleton intervals (for instance, `0` is the interval `[0, 0]`). Make use of the reflected operators as well.

## Template

```py
if __name__ == '__main__':
    i1 = Interval(int(input()), int(input()))
    i2 = Interval(int(input()), int(input()))
    print(i1 + i2)
    print(i1 - i2)
    print(i1 * i2)
    print(i1 / i2)
    print(i1 == i2)
    print(i1 != i2)
    print(i1 > i2)
    print(i1 >= i2)
    print(i1 < i2)
    print(i1 <= i2)
```

## Testcases

### 1

IN:
```
2
9
-4
1
```

OUT:
```
[-2.0, 10.0]
[1.0, 13.0]
[-36.0, 9.0]
[-inf, inf]
False
True
True
True
False
False
```

## Solution

```py
import math
class Interval:
  def __init__(self, low, high):
    self.low = float(low)
    self.high = float(high)

  def __str__(self):
    return f'[{self.low}, {self.high}]'

  def __add__(self, other):
    if isinstance(other, int):
      return Interval(self.low + other, self.high + other)
    return Interval(self.low + other.low, self.high + other.high)

  def __sub__(self, other):
    if isinstance(other, int):
      return Interval(self.low - other, self.high - other)
    return Interval(self.low - other.high, self.high - other.low)

  def __mul__(self, other):
    if isinstance(other, int):
      l = self.low * other
      h = self.high * other
      return Interval(min(l, h), max(l, h))
    ll = self.low * other.low
    lh = self.low * other.high
    hl = self.high * other.low
    hh = self.high * other.high
    return Interval(min(ll, lh, hl, hh), max(ll, lh, hl, hh))

  def __truediv__(self, other):
    if isinstance(other, int):
      if other == 0:
        return self * Interval(-math.inf, math.inf)
      else:
        operand = Interval(1.0/other, 1.0/other)
    if other.low > 0 or other.high < 0:
      operand = Interval(1.0/other.high, 1.0/other.low)
    elif other.high == 0:
      operand = Interval(-math.inf, 1.0/other.low)
    elif other.low == 0:
      operand = Interval(1.0/other.high, math.inf)
    else:
      operand = Interval(-math.inf, math.inf)
    return self * operand

  def __eq__(self, other):
    if isinstance(other, int):
      return self.low == other and self.high == other
    return isinstance(other, Interval) and self.low == other.low and self.high == other.high

  def __ne__(self, other):
    if isinstance(other, int):
      return self.low != other or self.high != other
    return not isinstance(other, Interval) or self.low != other.low or self.high != other.high

  def __lt__(self, other):
    if isinstance(other, int):
      return self.high < other
    return self.high < other.low

  def __le__(self, other):
    return self < other or self == other

  def __gt__(self, other):
    return not self <= other

  def __ge__(self, other):
    return not self < other

  def __iadd__(self, other):
    if isinstance(other, int):
      self.low += other
      self.high += other
    else:
      self.low += other.low
      self.high += other.high
    return self

  def __isub__(self, other):
    if isinstance(other, int):
      self.low -= other
      self.high -= other
    else:
      self.low -= other.high
      self.high -= other.low
    return self

  def __imul__(self, other):
    if isinstance(other, int):
      l = self.low * other
      h = self.high * other
      self.low = min(l, h)
      self.high = max(l, h)
    else:
      ll = self.low * other.low
      lh = self.low * other.high
      hl = self.high * other.low
      hh = self.high * other.high
      self.low = min(ll, lh, hl, hh)
      self.high = max(ll, lh, hl, hh)

  def __idiv__(self, other):
    if isinstance(other, int):
      other = Interval(other, other)
    if other.low > 0 or other.high < 0:
      operand = Interval(1.0/other.high, 1.0/other.low)
    elif other.high == 0:
      operand = Interval(-math.inf, 1.0/other.low)
    elif other.low == 0:
      operand = Interval(1.0/other.high, math.inf)
    else:
      operand = Interval(-math.inf, math.inf)
    self *= operand
    return self
```
