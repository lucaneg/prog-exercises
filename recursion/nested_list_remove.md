# Rimuovere un valore da una lista innestata

Scrivere una funzione `remove_value(lst, target)` che, data una lista `lst` possibilmente innestata (ovvero, ogni elemento potrebbe essere una lista a sua volta, e ogni elemento di questa sotto-lista potrebbe essere una sottolista a sua volta, ...) e un valore `target`, rimuove **tutte** le occorrenze di `target` in `lst` e in tutte le sue sotto-liste, ricorsivamente.

La funzione deve essere implementata ricorsivamente, e deve ritornare una lista costruita iterando su `lst`. Un elemento di `lst` viene aggiunto al risultato se (i) non è una lista e non è uguale a `target`, oppure (ii) se è una lista che, dopo essere stata "pulita" ricorsivamente dalle occorrenze di `target`, contiene almeno un elemento.

Link utili:
- [Documentazione Python](https://docs.python.org/3/)
- [Google Colab](https://colab.google.com)
- [Moodle](https://moodle.unive.it)

## Template

```py

```

## Testcases

### 1

IN:
```
546876
```

OUT:
```
[[7, 7, 0, [0, 6, [3, 0, 5, 5], 3, 10]],
 [4, 9, [8, [4, 6, 6, 3, 5, 6], [2, 3, 9, 6], 10, 9, 7], 5, 10],
 [7,
  [8, [10, 8], 3],
  [0, [4, 4, 10, 8, 5], [9, 3, 10, 2, 4]],
  4,
  0,
  [10, 6, 2, 2, 0],
  5,
  [[7, 4, 10, 0, 9], 6, 10, 0, 6, 9, 3]],
 2,
 5,
 [2, 3, [[6, 9, 0, 10, 5], [6, 5, 6], 4], [5, [3, 6, 6, 7], 5]],
 [[[4, 4, 0, 2, 5, 6], 2, 5], 7, 6, 0, 0]]
```

### 2

IN:
```
646848
```

OUT:
```
[[5, 4, [7, 1, [2, 4, 6, 9, 7, 2], 1, [7, 2, 4, 4, 9], 3], 5],
 3,
 [2,
  [[5, 8, 8, 4, 2, 6, 8], 10, [3, 4, 10, 3, 7, 7]],
  [[5, 10, 8, 10, 7, 6, 5, 3], 10, 8],
  8,
  [7, 4, 4, 10, 7, [9, 5, 3, 2]],
  6],
 1]
```

### 3

IN:
```
219877
```

OUT:
```
[6,
 4,
 [6,
  4,
  [[4, 2, 7, 5, 8, 3], 6, 1, 3],
  2,
  1,
  [[4, 6, 4, 6, 8], [8, 0, 1, 10], 1, [1, 2, 6], 5]]]
```

### 4

IN:
```
251687
```

OUT:
```
[9, 10, 4, 5, 3]
```

### 5

IN:
```
411568
```

OUT:
```
[[0,
  [[9, 5, 1, 7, 1, 3], 9, 7, 8, 1, 1],
  7,
  [8, 3, 9, [8, 8, 1, 8, 8, 6, 9], 9, 10],
  1,
  7],
 6,
 8,
 9]
```

## Solution

```py
def remove_value(lst, target):
  result = []
  for item in lst:
    if isinstance(item, list):
      cleaned = remove_value(item, target)
      if cleaned:
        result.append(cleaned)
    elif item != target:
      result.append(item)
  return result
```
