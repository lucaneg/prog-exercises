# Media mobile di un array

Dato un array, la sua media mobile di dimensione `k` è un secondo array della stessa dimensione che contiene, in ogni posizione, la media aritmetica dei `k` elementi centrati su quella posizione. Ad esempio, dato l'array `arr=[1, 6, -2, 1.5, -0.5]` e `k=3`, la media mobile di `arr` è così calcolata:
- per la posizione 0 vanno considerati gli elementi con indice 1, 0, e -1 (cioè l'ultimo elemento dell'array), quindi `(6+1-0.5)/3 = 2.1667`
- per la posizione 1 vanno considerati gli elementi con indice 2, 1, e 0, quindi `(-2+6+1)/3 = 1.6667`
- per la posizione 2 vanno considerati gli elementi con indice 3, 2, e 1, quindi `(1.5-2+6)/3 = 1.8333`
- per la posizione 3 vanno considerati gli elementi con indice 4, 3, e 2, quindi `(-0.5+1.5-2)/3 = -0.3333`
- per la posizione 4 vanno considerati gli elementi con indice 0, 4, e 3, quindi `(1-0.5+1.5)/3 = 0.6667`

L'array risultante è quindi `[2.1667, 1.6667, 1.8333, -0.3333, 0.6667]`. Scrivere una funzione `moving_average(arr, window_size)` che, dato un NumPy array monodimensionale e un numero intero, calcola la media mobile di ogni posizione dell'array. La funzione deve ritornare un array della stessa dimensione di quello ricevuto, in cui ogni elemento contiene la media mobile di dimensione window_size. E' possibile utilizzare **un solo ciclo**, da usare per iterare sulle posizioni dell'array risultato. Non è possibile utilizzare funzioni di NumPy che calcolino direttamente la media mobile.

Suggerimento: è possibile dividere il corpo del ciclo in casi: uno per quando la finestra da considerare è "lineare", uno per quando ci si trova all'inizio dell'array, e uno per quando ci si trova alla fine. 

Link utili:
- [Documentazione Python](https://docs.python.org/3/)
- [Documentazione NumPy](https://numpy.org/doc/)
- [Google Colab](https://colab.google.com)
- [Moodle](https://moodle.unive.it)

## Template

```py
if __name__ == '__main__':
  import numpy as np
  import random
  seed = int(input())
  random.seed(seed)
  k = random.randint(3, 8)
  if k % 2 == 0:
    k += 1
  n_terms = random.randint(15, 20)
  rng = np.random.default_rng(seed=seed)
  A = rng.integers(-50, 50, size=n_terms).astype(np.float64)
  print(' '.join(f'{round(e, 4)}' for e in moving_average(A, k)))
```

## Testcases

### 1

IN:
```
546876
```

OUT:
```
1.5556 9.3333 12.5556 20.7778 19.5556 15.5556 7.1111 12.2222 3.5556 0.8889 -3.4444 -11.4444 -16.6667 -10.3333 -6.1111 -1.0 -2.1111
```

### 2

IN:
```
646848
```

OUT:
```
-2.0 3.2 -3.2 13.8 -4.4 0.8 -5.0 13.6 8.2 23.2 26.6 27.2 15.0 3.4 6.6
```

### 3

IN:
```
219877
```

OUT:
```
-9.3333 6.0 20.3333 19.3333 7.0 -1.0 -17.0 -17.0 -21.3333 -14.6667 -2.3333 17.3333 17.3333 2.0 -19.6667
```

### 4

IN:
```
251687
```

OUT:
```
-7.2 3.6 -0.2 -9.0 -18.2 -15.4 -22.2 -26.8 -27.0 -25.2 -31.0 -25.2 -24.6 -20.2 -9.6 -11.2 -11.0 -4.4 0.8
```

### 5

IN:
```
411568
```

OUT:
```
5.4 -3.0 3.0 9.8 6.0 7.4 11.8 6.8 10.6 19.2 23.4 8.2 4.0 5.8 5.8 5.2 5.0 9.6
```

## Solution

```py
def moving_average(arr, window_size):
  half = (window_size - 1) // 2
  result = np.zeros_like(arr)
  for i in range(arr.size):
    if i - half >= 0:
      if i + half + 1 <= arr.size:
        total = arr[i-half : i+half+1].sum()
      else:
        total = arr[i-half :].sum() + arr[: i+half+1-arr.size].sum()
    else:
      total = arr[: i+half+1].sum() + arr[arr.size-(half-i) :].sum()
    result[i] = total / window_size
  return result
```
