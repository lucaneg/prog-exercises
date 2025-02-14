# Problema delle N regine

Negli scacchi, una regina può muovere e attaccare pezzi nemici lungo la riga, la colonna, e le diagonali su cui si trova, indipendentemente dalla distanza:

![img](https://brainking.com/images/rules/chess/07.gif)

Il problema delle N regine richiede, data una scacchiera NxN, di collocare N regine in modo che nessuna di esse sia sotto attacco.

Scrivere una funzione ricorsiva `place_queens(chessboard, n, remaining)` che, dati come parametri la scacchiera `chessboard` di dimensione `n`x`n`, la dimensione della scacchiera `n`, e il numero `remaining` di regine da collocare, usa il backtracking per trovare una soluzione al problema delle N regine.

Si noti che `chessboard` è un NumPy array che contiene `0` nelle celle libere e `1` nelle celle dove è presente una regina. Si proceda:
1. scrivendo una funzione per controllare se una cella è sotto attacco da una regina (non devono essere presenti `1` sulla riga, sulla colonna, e sulle due diagonali - non è necessario usare *esclusivamente* l'indexing di NumPy: si possono usare liberamente cicli)
2. la funzione `place_queens` che riempie la scacchiera sfruttando la funzione definita al punto 1 per utilizzare il backtracking. `place_queens` deve ritornare `None` se non esiste una soluzione. Le chiamate ricorsive decrementano il parametro `remaining`, e l'algoritmo si ferma quando `remaining` diventa 0.

## Template

```py
import numpy as np"

def is_under_attack_solution(chessboard, row, col):
  copy = chessboard.copy()
  copy[row, col] = 0
  n = copy.shape[0]
  if 1 in copy[row, :]:
    return True
  if 1 in copy[:, col]:
    return True

  for i in range(1, n):
    if row - i >= 0:
      if col - i >= 0 and copy[row - i, col - i] == 1:
        return True
      if col + i < n and copy[row - i, col + i] == 1:
        return True
    if row + i < n:
      if col - i >= 0 and copy[row + i, col - i] == 1:
        return True
      if col + i < n and copy[row + i, col + i] == 1:
        return True

  return False

def is_valid_solution(chessboard, n):
  queens = np.nonzero(chessboard == 1)
  if len(queens) == 0:
    return 'Non ci sono abbastanza regine (0)'
  if len(queens[0]) != n:
    return f'Non ci sono abbastanza regine ({len(queens[0])})'
  
  for row, col in zip(queens[0], queens[1]):
    if is_under_attack_solution(chessboard, row, col):
      return f'La regina in posizione {row},{col} è sotto attacco'

  return None

if __name__ == '__main__':
  n = int(input())
  chess = np.zeros((n, n), dtype=np.int8)
  solution = place_queens(chess, n, n)
  if solution is None:
    print('Nessuna soluzione')
  else:
    print('Soluzione valida')
```

## Testcases

### 1

IN:
```
1
```

OUT:
```
Soluzione valida
```

### 2

IN:
```
6
```

OUT:
```
Soluzione valida
```

### 3

IN:
```
5
```

OUT:
```
Soluzione valida
```

### 4

IN:
```
7
```

OUT:
```
Soluzione valida"
```

### 5

IN:
```
8
```

OUT:
```
Soluzione valida
```

### 6

IN:
```
2
```

OUT:
```
Nessuna soluzione
```

### 7

IN:
```
3
```

OUT:
```
Nessuna soluzione
```

### 8

IN:
```
4
```

OUT:
```
Soluzione valida
```

## Solution

```py
def is_under_attack(chessboard, row, col):
  copy = chessboard.copy()
  copy[row, col] = 0
  n = copy.shape[0]
  if 1 in copy[row, :]:
    return True
  if 1 in copy[:, col]:
    return True
  
  for i in range(1, n):
    if row - i >= 0:
      if col - i >= 0 and copy[row - i, col - i] == 1:
        return True
      if col + i < n and copy[row - i, col + i] == 1:
        return True
    if row + i < n:
      if col - i >= 0 and copy[row + i, col - i] == 1:
        return True
      if col + i < n and copy[row + i, col + i] == 1:
        return True

  return False

def place_queens(chessboard, n, remaining):
  if remaining == 0:
    return chessboard

  empty = np.nonzero(chessboard == 0)
  if len(empty) == 0:
    return None

  for row, col in zip(empty[0], empty[1]):
    if not is_under_attack(chessboard, row, col):
      c = chessboard.copy()
      c[row, col] = 1
      solution = place_queens(c, n, remaining - 1)
      if solution is not None:
        return solution

  return None
```
