# Campionamento di un polinomiale

Scrivere una funzione `generate_samples(min, max, n)` che ritorni un array di NumPy contenente `n` valori equidistanti compresi tra `min` e `max` (`min`, `max` e `n` sono tutti valori numerici).

Poi, scrivere una seconda funzione `sample_poly(x, coefficients)` che deve campionare un polinomio per determinati valori di `x`, contenuti nell'array di NumPy ottenuto come primo parametro. `coefficients` è un array di NumPy che riporta i coefficienti `a_0, a_1, a_2, ...` di ogni termine del polinomio, in ordine crescente di grado: `a_0` è il coefficiente di `x**0`, `a_1` è il coefficiente di `x**1`, `a_2` è il coefficiente di `x**2`, ... Ad esempio, se `coefficients = [4, 2, -3, 9]`, il polinomio corrispondente è `4 + 2x - 3x**2 + 9x**3`. `sample_poly` deve, per ogni coefficiente, calcolare la potenza di `x` corrispondente, moltiplicarla per il suo coefficiente, ed aggiungerla al risultato finale, che va poi ritornato.

E' consentito usare un solo ciclo for per iterare sui coefficienti.

## Template

```py
import numpy as np

def print_array(a):
  print(' ', ' '.join(f'{e:> #8.3f}' for e in a))

if __name__ == '__main__':
  min = int(input())
  max = int(input())
  n = int(input())
  c = [float(coeff) for coeff in input().split()]
  print_array(sample_poly(generate_samples(min, max, n), np.array(c, dtype=np.float64)))
```

## Testcases

### 1

IN:
```
-5
5
11
0 1
```

OUT:
```
    -5.000   -4.000   -3.000   -2.000   -1.000    0.000    1.000    2.000    3.000    4.000    5.000
```

### 2

IN:
```
-100
-67
42
0.5 8.3 -4.9 -17 1
```

OUT:
```
   116950170.500  113362919.420  109858710.559  106436254.425  103094271.600  99831492.735  96646658.556  93538519.861  90505837.518  87547382.471  84661935.734  81848288.393  79105241.609  76431606.613  73826204.709  71287867.273  68815435.754  66407761.674  64063706.625  61782142.274  59561950.359  57402022.691  55301261.152  53258577.698  51272894.357  49343143.229  47468266.486  45647216.374  43878955.208  42162455.380  40496699.350  38880679.653  37313398.896  35793869.759  34321114.991  32894167.417  31512069.934  30173875.509  28878647.184  27625458.071  26413391.358  25241540.300
```

### 3

IN:
```
0
1
1
4 3 -7
```

OUT:
```
     4.000
```

### 4

IN:
```
-100
100
50
0 -3.992 -5.3254 6.36
```

OUT:
```
  -6412854.800 -5661191.321 -4970683.955 -4338737.865 -3762758.214 -3240150.164 -2768318.879 -2344669.522 -1966607.254 -1631537.240 -1336864.641 -1079994.621 -858332.343 -669282.968 -510251.661 -378643.583 -271863.898 -187317.769 -122410.357 -74546.827 -41132.341 -19572.062 -7271.152 -1634.774  -68.092   23.732  1235.536  6162.156  17398.429  37539.193  69179.286  114913.543  177336.802  259043.901  362629.677  490688.967  645816.607  830607.436  1047656.291  1299558.007  1588907.424  1918299.378  2290328.706  2707590.245  3172678.832  3688189.306  4256716.502  4880855.258  5563200.412  6306346.800
```

### 5

IN:
```
-100
100
50
-27.12 -13.74 7.948 184.468 -70.784 -15.45
```

OUT:
```
   147237212826.880  119286076754.868  95749937012.301  76083924071.793  59789953435.347  46414625344.140  35547124488.319  26817119716.788  19892663747.000  14478092874.742  10311926683.932  7164767756.407  4837201381.710  3157695266.884  1980499246.259  1183544991.246  666345720.122  347895907.827  164570995.746  68027101.506  23100728.763  5708476.992  746751.279 -8527.891 -2214.843 -228.922 -189844.701 -2279982.194 -11601497.063 -39207470.831 -103973501.089 -234697991.707 -472202443.042 -871431742.153 -1503554453.004 -2458063106.680 -3844874491.592 -5796429943.691 -8469795636.673 -12048762872.195 -16745948370.079 -22804894558.526 -30502169864.323 -40149469003.054 -52095713269.312 -66729150826.904 -84479456999.065 -105819834558.667 -131269114018.427 -161393853921.120
```

## Solution

```py
def generate_samples(min, max, n):
  return np.linspace(min, max, n)

def sample_poly(x, coefficients):
  result = np.zeros_like(x)
  for i in range(len(coefficients)):
    result += coefficients[i] * x**i
  return result
```
