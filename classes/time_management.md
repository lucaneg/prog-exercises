# Managing time

We want to create a class to represent amounts of elapsed time, by separately storing hours, minutes, and seconds elapsed. The class should contain a function for increasing the stored time by a given amount, or by another time object. There should also be a way to pretty-print the stored time.

## Solution

```py
class Time:
  def __init__(self, hh, mm, ss):
    self.hours = hh
    self.minutes = mm
    self.seconds = ss

  def pretty_print(self):
    print(f'{self.hours}h:{self.minutes}m:{self.seconds}s')

  def increase_by_amount(self, hh, mm, ss):
    self.seconds += ss
    if self.seconds >= 60:
      self.minutes += self.seconds // 60
      self.seconds = self.seconds % 60
    self.minutes += mm
    if self.minutes >= 60:
      self.hours += self.minutes // 60
      self.minutes = self.minutes % 60
    self.hours += hh

  def increase_by_time(self, other):
    self.increase_by_amount(other.hours, other.minutes, other.seconds)

t1 = Time(1, 0, 25)
t1.pretty_print()
t2 = Time(0, 59, 51)
t2.pretty_print()
t1.increase_by_time(t2)
t1.pretty_print()
```
