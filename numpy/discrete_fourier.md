# Trasformata discreta di Fourier

La trasformata discreta di Fourier (DFT) converte una sequenza finita di campioni di un segnale periodico in coefficienti di sinusoidi complesse. Proprio come la trasformata di Fourier, la DFT è un modo per rappresentare un segnale nel dominio delle frequenze.

Dati `N` campioni `x_0,...x_(N-1)` di un segnale, la DFT produce `N` coefficienti `X_0,...X_(N-1)` tramite la formula:

![img](https://wikimedia.org/api/rest_v1/media/math/render/svg/01ca6ad7b791dfc2a33a2074acc03f58a53864ef)

Scrivere una funzione `dft(samples)` che, dato un array contenente `N` campioni di un segnale periodico, ritorna gli `N` coefficienti della DFT corrispondente utilizzando la formula data. I coefficienti devono essere ritornati come un array di numpy.

E' consentito usare un solo ciclo per risolvere l'esercizio, che deve iterare sugli indici `k` dei coefficienti. Il calcolo di un singolo coefficiente deve essere completamente vettorizzato.

## Template

```py
if __name__ == '__main__':
    import numpy as np
    n = int(input())
    samples = np.zeros(n, dtype=np.complex128)
    for i in range(n):
        samples[i] = complex(input())
    coeff = dft(samples)
    for c in coeff:
        print(f'{c.real:.8f} + {c.imag:.8f}j', end='   ')
```

## Testcases

### 1

IN:
```
5
3.14159+2.71828j
-1.23456+4.98765j
0.12345-0.54321j
-4.98765-1.23456j
2.71828+3.14159j
```

OUT:
```
-0.23889000 + 9.06975000j   9.69737789 + 7.42374449j   0.86568876 + 2.77659156j   0.01054092 + -11.59214047j   5.37323243 + 5.91345442j
```

### 2

IN:
```
8
4.56789+3.21098j
-2.34567-1.98765j
0.98765+4.12345j
-1.67890+2.34567j
1.23456-4.56789j
-3.45678+0.76543j
4.32109-2.45678j
-0.12345+1.67890j
```

OUT:
```
3.50639000 + 3.11211000j   10.32385871 + 9.00830670j   -4.75308000 + 0.97652000j   -6.60768661 + 7.17782495j   18.71599000 + -2.49259000j   9.50326129 + 13.21631330j   5.74050000 + -7.02368000j   0.11388661 + 1.71303505j
```

### 3

IN:
```
10
1.11111+2.22222j
-3.33333+4.44444j
2.12345-1.98765j
-4.87654+3.12345j
0.54321-0.98765j
4.44444+1.11111j
-2.34567-4.98765j
0.98765+3.21098j
-1.23456-2.12345j
3.87654+1.67890j
```

OUT:
```
1.29630000 + 5.70470000j   4.06336424 + 12.59460210j   6.55070019 + 5.76417396j   -1.51744860 + 8.45944597j   4.70925683 + 18.60733960j   -0.90122000 + -21.43306000j   6.58525050 + -10.80611984j   -14.32952562 + 3.18662586j   8.63624248 + -2.60344372j   -3.98182003 + 2.74793607j
```

### 4

IN:
```
9
-4.76543+0.12345j
2.98765-1.54321j
3.54321+4.76543j
-2.87654+0.98765j
1.65432-3.98765j
4.12345+2.65432j
-1.54321-4.32109j
0.12345+1.87654j
3.98765-2.12345j
```

OUT:
```
7.23455000 + -1.56801000j   3.53900432 + 0.66180313j   0.86754413 + -9.03598194j   -25.14650930 + 1.93497374j   4.18200914 + 11.35549601j   -14.77814751 + 8.82893845j   -9.64358070 + -9.99693374j   -1.59516735 + -3.18269619j   -7.54857273 + 2.11346053j
```

### 5

IN:
```
11
0.65432+4.56789j
-3.87654-1.43210j
1.12345+2.54321j
4.21098-3.98765j
-1.23456+0.43210j
2.98765-4.32109j
3.54321+0.65432j
-0.98765-3.87654j
1.43210+1.98765j
-4.32109+2.34567j
0.12345+4.98765j
```

OUT:
```
3.65532000 + 3.90111000j   -16.79655049 + 10.45097792j   -3.99665137 + 1.92983680j   -1.95094505 + 6.61807488j   10.62154861 + 8.11686201j   -19.25797715 + 7.96547604j   11.29267385 + 3.37419894j   15.69768349 + -4.83102730j   2.86000080 + -10.53050924j   7.16686305 + 2.40592512j   -2.09444573 + 20.84586484j
```

## Solution

```py
def dft(samples):
    N = len(samples)
    n = np.arange(N)
    coefficients = np.zeros_like(samples)
    for k in range(N):
        coefficients[k] = np.sum(samples * np.exp(-2j * np.pi * k * n / N))
    return coefficients
```
