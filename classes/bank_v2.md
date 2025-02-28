# Extending bank accounts

Starting from the solution to the bank account exercise, modify class `Account` by adding:
- a `fee` attribute, initialized in the constructor using a new parameter
- a `cost` attribute, initialized in the constructor using a new parameter
- a `deposit` method, that takes an amount of money as parameter, adds it to the current balance after removing the cost, and return the amount effectively deposited
- a `withdraw` method, that takes an amount of money as parameter, removes from the current balance the quantity `amount - (amount * fee)`, and returns the amount of money effectively withdrawn

Then, create three subclasses of `Account`:
- `RegularAccount`, that fixes `fee` to `0.03` and `cost` to `1.5`
- `PremiumAccount`, that fixes `fee` to `0.005` and `cost` to `0`
- `EmployeeAccount`, that fixes `fee` to `0.01` and `cost` to `0.3`

Finally, add methods in `Bank` to add the different types of accounts.

Also add an `__str__` method to all classes:
- `Owner.__str__` should return the string `'name surname'`
- `Account.__str__` should return the string `'owner (id): balance'`
- `Bank.__str__` should return the accounts separated by newlines

## Template

```py
if __name__ == '__main__':
    bank = Bank()

    print('Creating accounts')
    bank.add_owner(input(), input(), int(input()))
    bank.add_regular_account(bank.owners[0], int(input()))
    bank.accounts[0].balance = int(input())
    bank.add_premium_account(bank.owners[0], int(input()))
    bank.accounts[1].balance = int(input())
    print(bank)
    print()

    print('Moving money')
    q = bank.accounts[0].withdraw(int(input()))
    print(bank.accounts[1].deposit(q))
    print(bank)
    print()
```

## Testcases

### 1

IN:
```
Luca
Negrini
1
2
1000
3
100
500
```

OUT:
```
Creating accounts
Luca Negrini (2): 1000
Luca Negrini (3): 100

Moving money
485.0
Luca Negrini (2): 515.0
Luca Negrini (3): 585.0
```

## Solution

```py
class Owner:
  def __init__(self, name, surname, id):
    self.name = name
    self.surname = surname
    self.id = id

  def __str__(self):
    return f'{self.name} {self.surname}'

class Account:
  def __init__(self, id, balance, owner, fee, cost):
    self.id = id
    self.balance = balance
    self.owner = owner
    self.cost = cost
    self.fee = fee

  def deposit(self, amount):
    quantity = amount - self.cost
    self.balance += quantity
    return quantity

  def withdraw(self, amount):
    quantity = amount - (amount * self.fee)
    self.balance -= quantity
    return quantity

  def __str__(self):
    return f'{self.owner} ({self.id}): {self.balance}'

class RegularAccount(Account):
  def __init__(self, id, balance, owner):
    super().__init__(id, balance, owner, 0.03, 1.5)

class PremiumAccount(Account):
  def __init__(self, id, balance, owner):
    super().__init__(id, balance, owner, 0.005, 0.0)

class EmployeeAccount(Account):
  def __init__(self, id, balance, owner):
    super().__init__(id, balance, owner, 0.01, 0.3)

import random
class Bank:
  def __init__(self):
    self.owners = []
    self.accounts = []

  def add_owner(self, name, surname, id):
    self.owners.append(Owner(name, surname, id))

  def add_regular_account(self, owner, id):
    self.accounts.append(RegularAccount(id, 0, owner))

  def add_premium_account(self, owner, id):
    self.accounts.append(PremiumAccount(id, 0, owner))

  def add_employee_account(self, owner, id):
    self.accounts.append(EmployeeAccount(id, 0, owner))

  def __str__(self):
    return '\n'.join([str(a) for a in self.accounts])
```
