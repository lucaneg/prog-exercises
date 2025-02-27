# Identificare il tipo di una serie

Una sequenza di termini `x_0,...,x_n` costituisce una progressione aritmetica se la differenza tra termini successivi è costante, oppure costituisce una serie geometrica se il rapporto tra elementi successivi è costante.

Scrivere una funzione `kind(sequence)` che, data una sequenza di termini sotto forma di un array di numpy ritorna `True` se la sequenza è una progressione aritmetica, `False` se è una serie geometrica, e `None` altrimenti. Per farlo è necessario calcolare la differenza e il rapporto tra termini successivi della sequenza.

Non è consentito usare cicli.

## Template

```py
if __name__ == '__main__':
    import numpy as np
    n = int(input())
    sequence = np.zeros(n, dtype=np.float64)
    for i in range(n):
        sequence[i] = float(input())
    print(kind(sequence))
```

## Testcases

### 1

IN:
```
6
5
7
9
11
13
15
```

OUT:
```
True
```

### 2

IN:
```
6
-5
-7
-9
-11
-13
-15
```

OUT:
```
True
```

### 3

IN:
```
5
1
0.5
0.25
0.125
0.0625
```

OUT:
```
False
```

### 4

IN:
```
5
-1
-0.5
-0.25
-0.125
-0.0625
```

OUT:
```
False
```

### 5

IN:
```
10
1
-4
18
3
-20
65
12
0.2
-5.1
4
```

OUT:
```
None
```

## Solution

```py
def kind(sequence):
  head = sequence[:-1]
  tail = sequence[1:]
  diff = head-tail
  if np.all(diff == diff[0]):
    return True
  ratio = head/tail
  if np.all(ratio == ratio[0]):
    return False
  return None
```
