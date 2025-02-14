# Unione di liste ordinate

Scrivere una funzione ricorsiva `merge(l1, l2)` che, date due liste **ordinate crescentemente** `l1` e `l2`, ritorna una nuova lista **ordinata crescentemente** contenente tutti gli elementi di `l1` e `l2`. 

**Caso base:** se una delle due liste è vuota, `merge` deve ritornare l'altra lista.

**Caso ricorsivo:** `merge` identifica la lista il cui primo elemento è più piccolo, e lo estrae (rimuove) dalla lista. `merge` ritorna tale elemento seguito dal risultato della chiamata ricorsiva, passando come parametri le due liste aggiornate.

## Template

```py
if __name__ == '__main__':
  import random
  seed = int(input())
  min = int(input())
  max = int(input())
  count = int(input())

  random.seed(seed)
  rl1 = random.sample(range(min, max), count)
  rl2 = random.sample(range(min, max), count)
  expected = sorted(rl1 + rl2)
  actual = merge(sorted(rl1), sorted(rl2))
  print(expected == actual)
```

## Testcases

### 1

IN:
```
1
0
100
10
```

OUT:
```
True
```

### 2

IN:
```
98731834
-100
100
1
```

OUT:
```
True
```

### 3

IN:
```
321352468685498351
-100
100
50
```

OUT:
```
True
```

## Solution

```py
def merge(l1, l2):
  if len(l1) == 0:
    return l2
  if len(l2) == 0:
    return l1
  if l1[0] > l2[0]:
    e = l2[0]
    l2 = l2[1:]
  else:
    e = l1[0]
    l1 = l1[1:]
  return [e] + merge(l1, l2)
```
