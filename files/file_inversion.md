# Inversione di un file

Scrivere una funzione `invert(fin, fout)` che dato il nome di un file `fin` lo apre in lettura e legge tutto il suo contenuto. La funzione deve poi creare un file `fout`, per poi scrivere al suo interno il contenuto del primo file **invertendone le righe** (stampandole dall'ultima alla prima).

## Template

```py
if __name__ == '__main__':
    nlines = int(input())
    with open('/execution/test.txt', 'w') as myfile:
        for _ in range(nlines):
            myfile.write(input() + '\n')
    
    invert('/execution/test.txt', '/execution/out.txt')
    with open('/execution/out.txt') as myfile:
        print(myfile.read(), end='')
```

## Testcases

### 1

IN:
```
3
Ciao, my first file written from Python
---------------------------------
Hello world!
```

OUT:
```
Hello world!
---------------------------------
Ciao, my first file written from Python
```

### 2

IN:
```
1
Ciao, my first file written from Python
```

OUT:
```
Ciao, my first file written from Python
```

## Solution

```py
def invert(fin,fout):
    with open(fin) as f:
        lines = f.readlines()
    lines = lines[::-1]
    with open(fout, 'w') as f:
        for line in lines:
            f.write(line)
```
