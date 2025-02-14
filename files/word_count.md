# Conteggio parole

Scrivere una funzione `count(fname)` che dato il nome di un file `fname` ritorni il numero di parole contenute nel file.

## Template

```py
if __name__ == '__main__':
    nlines = int(input())
    with open('/execution/test.txt', 'w') as f:
        for _ in range(nlines):
            f.write(input() + '\n')
    print(count('/execution/test.txt'))
```

## Testcases

### 1

IN:
```
2
Ciao, my first file written from Python
Hello world!
```

OUT:
```
9
```

### 2

IN:
```
10
Ciao, my first file written from Python
Hello world!
Ciao, my first file written from Python
Hello world!
Ciao, my first file written from Python
Hello world!
Ciao, my first file written from Python
Hello world!
Ciao, my first file written from Python
Hello world!
```

OUT:
```
45
```

## Solution

```py
def count(fname):
    with open(fname) as f:
        content = f.read()
    return len(content.split())
```
