### Personaggi di un videogioco

Creare la classe `Character` che rappresenta un personaggio di un videogioco con un attributo `health` che viene inizializzato a `100` dal costruttore (**senza ricevere un parametro nel costruttore**). La classe deve implementare l'operatore `-=` che viene chiamato quando il personaggio subisce danni, e deve quindi sottrarre l'intero passato come parametro all'operatore da `health`.

Creare poi due classi: 
- `Warrior` che eredita da `Character` e:
  - contiene un attributo `strength` che è inizializzato tramite un parametro del costruttore;
  - definisce un metodo `attack(base_damage, attack_ratio)` che ritorna un numero pari a `base_damage` sommato a `attack_ratio` moltiplicato per la forza (attributo `strength` del personaggio);
- `Mage` che eredita da `Character` e:
  - contiene un attributo `ability` che è inizializzato tramite un parametro del costruttore;
  - contiene un attributo `mana` che è inizializzato tramite un parametro del costruttore;
  - definisce un metodo `cast(base_damage, ability_ratio, cost)` che ritorna un numero pari a `base_damage` sommato a `ability_ratio` moltiplicato per l'abilità (attributo `ability` del personaggio) **solo se `cost` è minore o uguale del mana attuale** (attributo `mana` del personaggio): se è minore o uguale il costo viene sottratto dal mana attuale, altrimenti viene lanciata una `ValueError`.

## Template

```py
if __name__ == '__main__':
  kind = input()
  if kind == 'w':
    strength = int(input())
    char = Warrior(strength)
    print(char.health, char.strength)
    
    nops = int(input())
    for _ in range(nops):
      line = input().split()
      if line[0] == 'A':
        print(char.attack(int(line[1]), float(line[2])))
      elif line[0] == 'D':
        char -= int(line[1])
        print(char.health)
      else:
        try:
          print(char.cast(int(line[1]), float(line[2]), int(line[3])))
        except Exception as e:
          print(e)
  else:
    ability = int(input())
    mana = int(input())
    char = Mage(ability, mana)
    print(char.health, char.ability, char.mana)
    
    nops = int(input())
    for _ in range(nops):
      line = input().split()
      if line[0] == 'A':
        try:
          print(char.attack(int(line[1]), float(line[2])))
        except Exception as e:
          print(e)
      elif line[0] == 'D':
        char -= int(line[1])
        print(char.health)
      else:
        try:
          print(char.cast(int(line[1]), float(line[2]), int(line[3])))
        except ValueError as e:
          print('no mana')
```

## Testcases

### 1

IN:
```
w
50
2
A 10 0.12
D 30
```

OUT:
```
100 50
16.0
70
```

### 2

IN:
```
m
70
120
3
C 70 0.3 100
D 48
C 55 0.6 20
```

OUT:
```
100 70 120
91.0
52
97.0
```

### 3

IN:
```
m
70
120
3
C 70 0.3 100
D 48
C 55 0.6 40
```

OUT:
```
100 70 120
91.0
52
no mana
```

### 4

IN:
```
w
50
1
C 10 0.12 20
```

OUT:
```
100 50
'Warrior' object has no attribute 'cast'
```

### 5

IN:
```
m
70
120
1
A 70 0.3
```

OUT:
```
100 70 120
'Mage' object has no attribute 'attack'
```

## Solution

```py
class Character:
  def __init__(self):
    self.health = 100

  def __isub__(self, other):
    self.health -= other
    return self

class Warrior(Character):
  def __init__(self, strength):
    super().__init__()
    self.strength = strength

  def attack(self, base_damage, attack_ratio):
    return base_damage + self.strength * attack_ratio

class Mage(Character):
  def __init__(self, ability, mana):
    super().__init__()
    self.ability = ability
    self.mana = mana

  def cast(self, base_damage, ability_ratio, cost):
    if cost > self.mana:
      raise ValueError()
    self.mana -= cost
    return base_damage + self.ability * ability_ratio
```
