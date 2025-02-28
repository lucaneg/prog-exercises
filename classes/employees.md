# Employees

Design a base class `Employee` with attributes `name`, `salary`, and `bonus_perc_per_project`. The class should also have a `calculate_bonus(completed_projects)` method that returns the salary multiplied by the percentage multiplied by the number of completed projects.

Create child classes `Manager` and `Engineer`. `Manager` should fix the percentage to 10%, while `Employee` should fix it to 2% but should receive a bonus only if at least 10 projects were completed.



## Template

```py
if __name__ == '__main__':
    manager = Manager(input(), float(input()))
    projects = int(input())
    print(f"{manager.name}'s bonus: ${manager.calculate_bonus(projects)}")
    engineer = Engineer(input(), float(input()))
    projects = int(input())
    print(f"{engineer.name}'s bonus: ${engineer.calculate_bonus(projects)}")
```

## Testcases

### 1

IN:
```
Alice
50000
15
Bob
60000
8
```

OUT:
```
Alice's bonus: $75000.0
Bob's bonus: $0
```

### 2

IN:
```
Alice
50000
1
Bob
60000
16
```

OUT:
```
Alice's bonus: $5000.0
Bob's bonus: $19200.0
```

## Solution

```py
class Employee:
  def __init__(self, name, salary, bonus_perc_per_project):
    self.name = name
    self.salary = salary
    self.bonus_perc_per_project = bonus_perc_per_project

  def calculate_bonus(self, completed_projects):
    return self.salary * (self.bonus_perc_per_project * completed_projects)

class Manager(Employee):
  def __init__(self, name, salary):
    super().__init__(name, salary, 0.1)

class Engineer(Employee):
  def __init__(self, name, salary):
    super().__init__(name, salary, 0.02)

  def calculate_bonus(self, completed_projects):
    if completed_projects >= 10:
      return super().calculate_bonus(completed_projects)
    else:
      return 0
```
