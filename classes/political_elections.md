# Political elections

Write a class `Party` that represents a political party. A party has a `name`, a list containing the names of `members`, and the name of a `candidate`. The list of members **is fixed and is defined during the creation of objects from this class**.

Then, define a class `Vote` that represents a vote a citizen has casted during an election. A vote should contain a `voted` attribute that holds the `Party` that has been voted, as well as the `date` (`datetime.date`) it was performed on and the `district` where the vote happened (as a string). Then, write an `IndividualVote` class that inherits from `Vote` and stores the name of a `party_member` that received the vote. `IndividualVote` represents a vote given to a specific member of a party, instead of the main candidate: its constructor should raise a `ValueError` if the `party_member` is not among the `members` of the `voted` party.

Finally, write a class `Election` that contains a list of `votes`. **The list starts empty**, and can be filled by adding a new vote with the in-place variant of `__add__` (that should also check if the received object has type `Vote`, raising a `TypeError` if it has a different type). `Election` should also have a `get_votes_for(candidate_name)` method that returns the number of votes for the given candidate:
- normal `Vote`s (the ones for a whole party) count as a vote for the `candidate` of that party
- `IndividualVote`s (the ones for a single person) count as a vote for the `party_member` voted

All attributes should be initialized inside constructors. `Party`, `Vote`, and `IndividualVote` should also define a way to convert objects of their type into strings.

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
  n = int(input())
  parties = dict()
  for i in range(n):
    pstr = input().split()
    parties[pstr[0]] = Party(pstr[0], pstr[1:-1], pstr[-1])

  election = Election()
  n = int(input())
  date_format = '%Y-%m-%d'
  for i in range(n):
    vstr = input().split()
    try:
      if vstr[0] == 'individual':
        election += IndividualVote(parties[vstr[1]], datetime.datetime.strptime(vstr[2], date_format), vstr[3], vstr[4])
      else:
        election += Vote(parties[vstr[1]], datetime.datetime.strptime(vstr[2], date_format), vstr[3])
    except Exception as e:
      print('Exception raised')
      import sys
      sys.exit()

  candidates = input().split()
  for c in candidates:
    print(election.get_votes_for(c))
```

## Testcases

### 1

IN:
```
2
Fratelli_d'_Italia Tommaso_Foti Messina_Manilo Elisabetta_Gardini Giorgia_Meloni
Partito_Democratico Chiara_Braga Bonafe_Simona Paolo_Ciani Elly_Schlein
4
general Fratelli_d'_Italia 2024-04-16 Verona
general Partito_Democratico 2024-04-16 Verona
individual Fratelli_d'_Italia 2024-04-16 Venezia Luca_Negrini
individual Partito_Democratico 2024-04-16 Venezia Chiara_Braga
Giorgia_Meloni Chiara_Braga Luca_Negrini
```

OUT:
```
Exception raised
```

### 2

IN:
```
2
Fratelli_d'_Italia Tommaso_Foti Messina_Manilo Elisabetta_Gardini Giorgia_Meloni
Partito_Democratico Chiara_Braga Bonafe_Simona Paolo_Ciani Elly_Schlein
4
general Fratelli_d'_Italia 2024-04-16 Verona
general Partito_Democratico 2024-04-16 Verona
individual Fratelli_d'_Italia 2024-04-16 Venezia Messina_Manilo
individual Partito_Democratico 2024-04-16 Venezia Chiara_Braga
Giorgia_Meloni Chiara_Braga Luca_Negrini
```

OUT:
```
1
1
0
```

### 3

IN:
```
2
Fratelli_d'_Italia Tommaso_Foti Messina_Manilo Elisabetta_Gardini Giorgia_Meloni
Partito_Democratico Chiara_Braga Bonafe_Simona Paolo_Ciani Elly_Schlein
12
general Fratelli_d'_Italia 2024-04-16 Verona
general Partito_Democratico 2024-04-16 Verona
general Partito_Democratico 2024-04-16 Verona
general Partito_Democratico 2024-04-16 Verona
general Partito_Democratico 2024-04-16 Verona
general Fratelli_d'_Italia 2024-04-16 Verona
general Fratelli_d'_Italia 2024-04-16 Verona
individual Fratelli_d'_Italia 2024-04-16 Verona Giorgia_Meloni
individual Fratelli_d'_Italia 2024-04-16 Venezia Messina_Manilo
individual Fratelli_d'_Italia 2024-04-16 Venezia Messina_Manilo
individual Fratelli_d'_Italia 2024-04-16 Venezia Elisabetta_Gardini
individual Partito_Democratico 2024-04-16 Venezia Chiara_Braga
Giorgia_Meloni Chiara_Braga Luca_Negrini Elisabetta_Gardini Messina_Manilo Elly_Schlein
```

OUT:
```
4
1
0
1
2
4
```

## Solution

```py
class Party:
  def __init__(self, name, members, candidate):
    self.name = name
    self.members = members
    self.candidate = candidate

  def __str__(self):
    return f'{self.name} with main candidate {self.candidate} (members: {" ,".join(self.members)})'

class Vote:
  def __init__(self, voted, date, district):
    self.voted = voted
    self.date = date
    self.district = district

  def __str__(self):
    return f'Vote for the main candidate of {self.voted}, registered on {self.date} in district {self.district}'

class IndividualVote(Vote):
  def __init__(self, voted, date, district, party_member):
    super().__init__(voted, date, district)
    if party_member not in voted.members and party_member != voted.candidate:
      raise ValueError(f'{party_member} is not part of {voted.name}')
    self.party_member = party_member

  def __str__(self):
    return f'Vote for the {self.party_member} of {self.voted}, registered on {self.date} in district {self.district}'

class Election:
  def __init__(self):
    self.votes = []

  def __iadd__(self, vote):
    if not isinstance(vote, Vote):
      raise TypeError(f'{vote} is not a valid vote')
    self.votes.append(vote)
    return self
  
  def get_votes_for(self, candidate_name):
    count = 0
    for vote in self.votes:
      if isinstance(vote, IndividualVote) and vote.party_member == candidate_name:
        count += 1
      elif not isinstance(vote, IndividualVote) and vote.voted.candidate == candidate_name:
        count += 1
    return count
```
