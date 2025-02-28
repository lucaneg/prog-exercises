# Bank

A bank keeps track of a number of accounts, each with a single owner. Write:
- a class `Owner` representing the person that owns accounts in the bank. An owner should have a name, a surname, and an ID (an integer assigned from the bank to identify the person)
- a class `Account` representing a single bank account, having an ID, the amount of money on it, and an owner
- a class `Bank`, storing a list of owners and a list of accounts

All classes should initialize their attributes in a constructor:
- `Owner` and `Account` should receive values for their attributes as constructor parameters
- `Bank` should initialize both owners and accounts to an empty list

Finally, `Bank` should have two methods:
- `add_owner`, that takes a name, a surname, and an ID, generates an `Owner` instance, and stores it in the list of owners
- `add_account` that takes an `Owner` and an ID and creates an `Account` with the given ID and no money in it, and stores it in the list of accounts
- `show_accounts` that, for each account currently in the bank, prints the string 'name surname (id): balance' where name and surname are the attributes of the owner, while id and balance are the attributes of the account

## Template

```py
if __name__ == '__main__':
    bank = Bank()

    print('Creating first owner and account')
    bank.add_owner(input(), input(), int(input()))
    bank.add_account(bank.owners[0], int(input()))
    bank.show_accounts()
    print()

    print('Adding money to the account')
    bank.accounts[0].balance = int(input())
    bank.show_accounts()
    print()

    print('Adding a second account')
    bank.add_account(bank.owners[0], int(input()))
    bank.show_accounts()
    print()

    print('Changing the money on the first account')
    bank.accounts[0].balance = int(input())
    bank.show_accounts()
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
100
3
40
```

OUT:
```
Creating first owner and account
Luca Negrini (2): 0

Adding money to the account
Luca Negrini (2): 100

Adding a second account
Luca Negrini (2): 100
Luca Negrini (3): 0

Changing the money on the first account
Luca Negrini (2): 40
Luca Negrini (3): 0
```

## Solution

```py
class Owner:
  def __init__(self, name, surname, id):
    self.name = name
    self.surname = surname
    self.id = id

class Account:
  def __init__(self, id, balance, owner):
    self.id = id
    self.balance = balance
    self.owner = owner

class Bank:
  def __init__(self):
    self.owners = []
    self.accounts = []

  def add_owner(self, name, surname, id):
    self.owners.append(Owner(name, surname, id))

  def add_account(self, owner, id):
    self.accounts.append(Account(id, 0, owner))

  def show_accounts(self):
    for account in self.accounts:
      print(f'{account.owner.name} {account.owner.surname} ({account.id}): {account.balance}')
```
