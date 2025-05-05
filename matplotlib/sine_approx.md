# Approssimare sin(x)

Scrivere una funzione `sine_series(x_min, x_max, n_samples, n_terms)` che approssima la funzione seno utilizzando la serie di Taylor:

![img](https://wikimedia.org/api/rest_v1/media/math/render/svg/94cebd1a5ee8ee760f179146f5a960e42d4400f3)

La funzione deve generare i valori di `x` per cui campionare la funzione suddividendo in `n_samples` parti uguali l'intervallo `[x_min, x_max]`, per poi procedere a trasformarli usando la serie di Taylor dove il parametro `n_terms` della funzione è il punto di arresto della sommatoria, utilizzando operazioni vettorializzate. E' possibile utilizzare un solo ciclo for, che deve iterare sull'indice `n` della sommatoria.

La funzione deve ritornare l'array contenente i samples calcolati. Prima di farlo, deve creare un grafico dove:
- il valore vero del seno, ottenibile tramite la funzione `np.sin(samples)` (dove `samples` sono i valori ottenuti campionando l'intervallo), deve essere visualizzato come una linea continua di colore verde;
- il valore approssimato del seno, calcolato tramite la serie di Taylor, deve essere visualizzato come una linea tratteggiata di colore rosso, con dei triangoli in corrispondenza dei campioni.

Il grafico deve avere un titolo, etichette per entrambi gli assi, e la legenda deve essere visibile.

NB: su avogador non è possibile visualizzare i grafici: provate il vostro codice su Colab per controllare se la creazione è corretta.

## Template

```py
if __name__ == '__main__':
  def print_array(arr):
    print(' ', ' '.join(f'{e:> #8.5f}' for e in arr))

  x_min = int(input())
  x_max = int(input())
  n_samples = int(input())
  n_terms = int(input())
  
  print_array(sine_series(x_min, x_max, n_samples, n_terms)[:10])
```

## Testcases

### 1

IN:
```
0
10
50
10
```

OUT:
```
   0.00000  0.20267  0.39692  0.57471  0.72863  0.85232  0.94063  0.98990  0.99809  0.96485
```

### 2

IN:
```
0
10
50
30
```

OUT:
```
   0.00000  0.20267  0.39692  0.57471  0.72863  0.85232  0.94063  0.98990  0.99809  0.96485
```

### 3

IN:
```
-10
10
100
50
```

OUT:
```
   0.54402  0.36460  0.17035 -0.03083 -0.23076 -0.42130 -0.59471 -0.74392 -0.86288 -0.94674
```

### 4

IN:
```
-40
40
100
80
```

OUT:
```
   0.20579 -0.70667 -1.03274  0.07521  0.86644  0.92816  0.54905 -0.21622 -0.85702 -0.96709
```

## Solution

```py
import numpy as np
import math
import matplotlib.pyplot as plt

def sine_series(x_min, x_max, n_samples, n_terms):
  samples = np.linspace(x_min, x_max, n_samples)
  sine_approx = np.zeros_like(samples)
  for n in range(n_terms):
    term = ((-1)**n * samples**(2*n+1)) / math.factorial(2*n + 1)
    sine_approx = sine_approx + term
  fig, ax = plt.subplots()
  ax.plot(samples, np.sin(samples), label="True Sine", color='green')
  ax.plot(samples, sine_approx, '^--r', label=f"Approx Sine (n={n_terms})")
  ax.legend()
  ax.set_xlabel("x")
  ax.set_ylabel("sin(x)")
  ax.set_title("Sine Function Approximation using Taylor Series")

  return sine_approx
```
