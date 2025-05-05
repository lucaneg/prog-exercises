# Differenze tra file binari

Scrivere una funzione `patch(file1, file2, output)` che legge due file **binari** e crea un terzo file **di testo** che ne contiene le differenze. `file1`, `file2` e `output` sono percorsi (ad esempio `cartella/nome_file.bin`) che specificano dove si trovano i file da leggere e dove va scritto il file di risultato. La funzione deve aprire i file corrispondenti usando i permessi appropriati, leggere `file1` e `file2` 5 bytes alla volta, e **solo se le sequenze di 5 byte sono diverse** li deve aggiungere ad `output` scrivendo `i: bytes_di_file1 --- bytes_di_file2` (`i` è un contatore che tiene traccia di quante sequenze di 5 bytes sono state lette finora). Notare che se uno dei due file è più corto dell'altro, i byte in eccesso vanno stampati come `i: bytes_di_file1 --- no data` (nel caso in cui il primo file sia più lungo del secondo, altrimenti `no data` va inserito prima dei tre trattini).

NB: quando si scrive un byte in un file di testo, si ottiene la notazione `\x0A`. E' invece richiesta la notazione esadecimale: bisogna quindi convertire ogni sequenza di 5 bytes in intero come visto a lezione, per poi stamparlo usando la stringa di formato `02X`. Ad esempio, se la variabile `v` contiene il risultato della conversione da bytes a intero, la possiamo convertire in stringa usando `f'{v:02X}'`.

## Template

```py
if __name__ == '__main__':
  import random
  seed = int(input())
  random.seed(seed)

  len1 = random.randint(5, 15)
  len2 = random.randint(len1-2, len1+2)

  with open('/execution/f1.bin', 'wb') as f1:
    with open('/execution/f2.bin', 'wb') as f2:
      for i in range(max(len1, len2)):
        bb1 = random.randbytes(5)
        bb2 = random.randbytes(5)
        if i < len1:
          f1.write(bb1)
        if i < len2:
          if random.choice([True, True, True, False]):
            f2.write(bb1)
          else:
            f2.write(bb2)

  patch('/execution/f1.bin', '/execution/f2.bin', '/execution/patch.txt')

  with open('/execution/patch.txt') as out:
    print(out.read())
```

## Testcases

### 1

IN:
```
0
```

OUT:
```
0: 420A5D2F34 --- 82F728B4FA
6: 8F78DE5857 --- 5A19C78DF4
11: no data --- 7E9CA5499D
```

### 2

IN:
```
1
```

OUT:
```
1: 7E1E2FEB89 --- 73C2CE6F44
3: 77CE42C82 --- D5E4B06CE6
7: no data --- 507D4BEDC
8: no data --- E1F06C144A
```

### 3

IN:
```
2
```

OUT:
```
2: AE94C9C950 --- FF288BC781
3: 64A372DB8F --- no data
4: FEDC38F519 --- no data
```

### 4

IN:
```
3
```

OUT:
```
0: 218B529B4A --- EA5EB561A4
3: FE31162427 --- 78B7970386
5: 26A2863A7F --- EDDE383784
8: no data --- C74D1FE09F
9: no data --- EBB804D820
```

### 5

IN:
```
4
```

OUT:
```
7: D816332ACA --- 569B191BF4
```

## Solution

```py
import sys

def read_from(f):
  data = f.read(5)
  v = int.from_bytes(data, sys.byteorder)
  return len(data) != 0, v

def patch(file1, file2, output):
  with open(file1, 'rb') as f1:
    with open(file2, 'rb') as f2:
      with open(output, 'w') as out:
        idx = 0
        while True:
          has_read_1, v1 = read_from(f1)
          has_read_2, v2 = read_from(f2)
          if not has_read_1 and not has_read_2:
            break
          elif not has_read_1:
            out.write(f'{idx}: no data --- {v2:02X}\n')
          elif not has_read_2:
            out.write(f'{idx}: {v1:02X} --- no data\n')
          elif v1 != v2:
              out.write(f'{idx}: {v1:02X} --- {v2:02X}\n')
          idx += 1
```
