# Scacchiera

Scrivere una funzione `checkboard(n)` che crea un array 2D di NumPy contenente i numeri consecutivi da 1 a `n*n`, organizzati in una matrice `n x n`. Questa funzione deve poi creare un pattern a scacchiera, impostando a 0 gli elementi dell'array corrispondenti alle celle nere della scacchiera:
- sulle righe dispari (1, 3, 5, ...) vanno impostati a 0 gli elementi in posizione dispari
- sulle righe pari (0, 2, 4, ...) vanno impostati a 0 gli elementi in posizione pari

## Template

```py
if __name__ == '__main__':
    n = int(input())
    board = checkboard(n)
    for r in range(board.shape[0]):
        s = ''
        for c in range(board.shape[1]):
            s += str(board[r,c]) + ' '
        print(s.strip())
```

## Testcases

### 1

IN:
```
2
```

OUT:
```
0 2
3 0
```

### 2

IN:
```
4
```

OUT:
```
0 2 0 4
5 0 7 0
0 10 0 12
13 0 15 0
```

### 3

IN:
```
6
```

OUT:
```
0 2 0 4 0 6
7 0 9 0 11 0
0 14 0 16 0 18
19 0 21 0 23 0
0 26 0 28 0 30
31 0 33 0 35 0
```

### 4

IN:
```
8
```

OUT:
```
0 2 0 4 0 6 0 8
9 0 11 0 13 0 15 0
0 18 0 20 0 22 0 24
25 0 27 0 29 0 31 0
0 34 0 36 0 38 0 40
41 0 43 0 45 0 47 0
0 50 0 52 0 54 0 56
57 0 59 0 61 0 63 0
```

## Solution

```py
import numpy as np
def checkboard(n):
    # first we generate all the elements in the checkboard
    target = np.arange(1, (n**2)+1)
    # then we give the board a squared shape
    target = target.reshape(n, n)
    
    # we start from the even rows: we take all of them starting at 0 with step 2
    # and in them, we change all elements starting at 0 with step 2 to 0
    target[::2, ::2] = 0

    # we continue with odd rows: we take all of them starting at 1 with step 2
    # and in them, we change all elements starting at 1 with step 2 to 0
    target[1::2, 1::2] = 0

    return target
```
