# Metodi di pagamento

Creare una classe `BankAccount` che modella un conto bancario. Un conto è descritto da un unico attributo `balance`, che viene inizializzato nel costruttore tramite un apposito parametro, e che rappresenta la quantità di denaro attualmente sul conto. Tale classe definisce due modi di modificare `balance`, entrambi definiti attraverso operatori: la variante in-place di `__add__` permette di aggiungere una quantità di denaro, mentre la variante in-place di `__sub__` permette di sottrarre denaro. Entrambi gli operatori devono lanciare una `InvalidAmountException` se la quantità ricevuta non è numerica (`float` o `int`).

Definire poi la classe `CreditCard`, che rappresenta una carta di credito collegata a un conto. Tale classe ha due attributi: un conto bancario `account` e una tassa di pagamento `tax`, entrambi inizializzati tramite parametri del costruttore. La classe definisce inoltre due metodi: `pay` che riceve una quantità `amount` di denaro e usa uno dei due operatori definiti in `BankAccount` per sottrarre `amount + (amount * tax)` dal conto, e `refund` che riceve come parametro una quantità `amount` da rimborsare e lancia sempre una `RefundNotSupportedException`.

Infine, definire la classe `PayPal` che eredita da `CreditCard`. Questa classe fissa la tassa a 0 nel suo costruttore, mentre riceve ancora l'`account` a cui collegarsi come parametro. `PayPal` ridefinisce (cioè fa l'override) `refund` per riaccreditare la quantità `amount` sul conto, sfruttando uno dei due operatori definiti su `BankAccount`.

## Template

```py
class InvalidAmountException(Exception):
  pass

class RefundNotSupportedException(Exception):
  pass

if __name__ == '__main__':
  def print_balance(b):
    print(f'{b:.3f}')

  balance = float(input())
  type_and_tax = input()
  if type_and_tax != 'p':
    tax = float(type_and_tax.split()[1])

  account = BankAccount(balance)
  if type_and_tax == 'p':
    payment = PayPal(account)
  else:
    payment = CreditCard(account, tax)

  n = int(input())
  for _ in range(n):
    print_balance(account.balance)
    op, amount = input().split()
    try: 
      amount = float(amount)
    except:
      pass
    if op == 'p':
      try:
        payment.pay(amount)
      except InvalidAmountException:
        print('invalid amount')
    else:
      try:
        payment.refund(amount)
      except InvalidAmountException:
        print('invalid amount')
      except RefundNotSupportedException:
        print('refund not supported')
  print_balance(account.balance)
```

## Testcases

### 1

IN:
```
100
c 0.05
2
p 10 
r 17
```

OUT:
```
100.000
89.500
refund not supported
89.500
```

### 2

IN:
```
100
p
2
p f
r 7
```

OUT:
```
100.000
invalid amount
100.000
107.000
```

### 3

IN:
```
100
p
2
p 10
r 7
```

OUT:
```
100.000
90.000
97.000
```

### 4

IN:
```
100
p
4
r 37.2
p 102.51
r 4
r 27.88
```

OUT:
```
100.000
137.200
34.690
38.690
66.570
```

### 5

IN:
```
100
c 0.01
5
p 7.2
p 8.4
p 53.12
r 10
p 1.73
```

OUT:
```
100.000
92.728
84.244
30.593
refund not supported
30.593
28.846
```

## Solution

```py
class BankAccount:
  def __init__(self, initial_balance):
    self.balance = initial_balance

  def __iadd__(self, other):
    if not isinstance(other, int) and not isinstance(other, float):
      raise InvalidAmountException()
    self.balance += other
    return self

  def __isub__(self, other):
    if not isinstance(other, int) and not isinstance(other, float):
      raise InvalidAmountException()
    self.balance -= other
    return self

class CreditCard:
  def __init__(self, account, tax):
    self.account = account
    self.tax = tax

  def pay(self, amount):
    self.account -= amount + (amount * self.tax)

  def refund(self, amount):
    raise RefundNotSupportedException()

class PayPal(CreditCard):
  def __init__(self, account):
    super().__init__(account, 0)

  def refund(self, amount):
    self.account += amount
```
