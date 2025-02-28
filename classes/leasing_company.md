# Leasing company

Define a `Lease` class representing the lease of a car to someone. A lease is described by the `brand` of the car, the `model` of the car, the `starting_date` (`datetime.date`) of the lease, and the monthly `price` of the lease. This class should also have a `get_price` method that returns the `price` provided when constructing the object.

Then, define two subclasses of `Lease`: `PersonalLease` and `CorporateLease`. `PersonalLease` represents a lease contract made with a single person, while `CorporateLease` represents a lease contract made with a company. A `PersonalLease` also stores the `name` of the person and a `discount` percentage, and will override `get_price` to return `price - (price * discount / 100)`. `CorporateLease` will instead store the `company` name.

Conclude by defining a `LeasingCompany` class that contains a list of leases **that starts empty**, and it is filled with the in-place `__add__`. This operation should also check that an individual does not lease more than one car: if a `PersonalLease` is received and there is already a lease for the same person, the method should raise a `ValueError`. `LeasingCompany` should also define a method called `calculate_income_after` that takes a parameter `start_date` (`datetime.date`) and computes the monthly income of the company during the given time frame, by summing the results of `get_price` of the leases that started after `start_date`.

All attributes should be initialized inside constructors. All classes should define the string conversion operator.

You can operate with dates by through the `datetime` module:
```python
import datetime
fifteen_jan = datetime.date(2024, 01, 15) # year, month, day
today = datetime.date.today()
print(fifteen_jan < today) # you can compare dates with <, <=, ==, ...
```

## Template

```py
import datetime

if __name__ == '__main__':
  company = LeasingCompany()
  n = int(input())
  date_format = '%Y-%m-%d'
  for i in range(n):
    lstr = input().split()
    try:
      if lstr[0] == 'corporate':
        company += CorporateLease(lstr[1], lstr[2], datetime.datetime.strptime(lstr[3], date_format), int(lstr[4]), lstr[5])
      else:
        company += PersonalLease(lstr[1], lstr[2], datetime.datetime.strptime(lstr[3], date_format), int(lstr[4]), lstr[5], float(lstr[6]))
    except Exception as e:
      print('Exception raised')
      import sys
      sys.exit()

  date = datetime.datetime.strptime(input(), date_format)
  print(company.calculate_income_after(date))
  for l in company.leases: 
    print(l.get_price())
```

## Testcases

### 1

IN:
```
3
personal VW Golf 2024-04-16 350 Luca_Negrini 4.1
personal Fiat Tipo 2024-04-16 250 Pietro_Ferrara 2.8
corporate Audi A4 2024-04-16 500 Alpenite
2024-04-15
```

OUT:
```
1078.65
335.65
243.0
500
```

### 2

IN:
```
3
personal VW Golf 2024-04-16 350 Luca_Negrini 4.1
personal Fiat Tipo 2024-04-16 250 Luca_Negrini 2.8
corporate Audi A4 2024-04-16 500 Alpenite
2024-04-15
```

OUT:
```
Exception raised
```

### 3

IN:
```
5
personal VW Golf 2024-04-16 350 Luca_Negrini 4.1
personal VW Golf 2024-03-16 350 Luca_Olivieri 2.1
personal VW Golf 2024-03-16 350 Greta_Dolcetti 6.5
personal Fiat Tipo 2024-04-16 250 Pietro_Ferrara 2.8
corporate Audi A4 2024-03-16 500 Alpenite
2024-04-15
```

OUT:
```
578.65
335.65
342.65
327.25
243.0
500
```

## Solution

```python
class Lease:
  def __init__(self, brand, model, starting_date, price):
    self.brand = brand
    self.model = model
    self.starting_date = starting_date
    self.price = price

  def get_price(self):
    return self.price

  def __str__(self):
    return f'Leasing a {self.brand} {self.model} for {self.get_price()} euro/month starting on {self.starting_date}'

class PersonalLease(Lease):
  def __init__(self, brand, model, starting_date, price, name, discount):
    super().__init__(brand, model, starting_date, price)
    self.name = name
    self.discount = discount

  def get_price(self):
    return self.price - (self.price * self.discount / 100)

  def __str__(self):
    return super().__str__() + f' to {self.name}'

class CorporateLease(Lease):
  def __init__(self, brand, model, starting_date, price, company):
    super().__init__(brand, model, starting_date, price)
    self.company = company

  def __str__(self):
    return super().__str__() + f' to company {self.company}'

class LeasingCompany:
  def __init__(self):
    self.leases = []

  def __str__(self):
    return '\n'.join([str(l) for l in self.leases])

  def __iadd__(self, lease):
    if isinstance(lease, PersonalLease):
      for l in self.leases:
        if isinstance(l, PersonalLease) and lease.name == l.name:
          raise ValueError(f'{lease.name} already has a lease!')
    
    self.leases.append(lease)
    return self

  def calculate_income_after(self, start_date):
    income = 0
    for l in self.leases:
      if l.starting_date >= start_date:
        income += l.get_price()
    return income
```
