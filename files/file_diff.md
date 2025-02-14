# Differenze tra file

Scrivere una funzione `diff(f1, f2)` che, dati i nome di due file `f1` e `f2`, li legge **una riga alla volta** e li confronta:
- quando due righe sono uguali, vengono saltate
- quando due righe sono diverse, vengono stampate con `f1: linea1\nf2: linea2`
- quando uno dei due file termina, le linee dell'altro file devono essere stampate con `f1 only: linea` (usando f2 se le righe sono nel secondo file)

## Template

```py
if __name__ == '__main__':
    nlines = int(input())
    with open('/execution/test1.txt', 'w') as myfile:
        for _ in range(nlines):
            myfile.write(input() + '\n')
    nlines = int(input())
    with open('/execution/test2.txt', 'w') as myfile:
        for _ in range(nlines):
            myfile.write(input() + '\n')
    
    diff('/execution/test1.txt', '/execution/test2.txt')
```

## Testcases

### 1

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
10
Ciao, my first file written from Python
Hello worlda!
Ciao, myd first file written from Python
Hello world!
Ciao, my first file written from Python
Hello world!
Ciao, my first file written from Python
Hello world!
Ciao, my first file writtenff from Python
Hello world!
```

OUT:
```
f1: Hello world!
f2: Hello worlda!
f1: Ciao, my first file written from Python
f2: Ciao, myd first file written from Python
f1: Ciao, my first file written from Python
f2: Ciao, my first file writtenff from Python
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
8
Ciao, my first file written from Python
Hello worlda!
Ciao, myd first file written from Python
Hello world!
Ciao, my first file written from Python
Hello world!
Ciao, my first file written from Python
Hello world!
```

OUT:
```
f1: Hello world!
f2: Hello worlda!
f1: Ciao, my first file written from Python
f2: Ciao, myd first file written from Python
f1 only: Ciao, my first file written from Python
f1 only: Hello world!
```

### 3

IN:
```
6
Ciao, my first file written from Python
Hello world!
Ciao, my first file written from Python
Hello world!
Ciao, my first file written from Python
Hello world!
8
Ciao, my first file written from Python
Hello worlda!
Ciao, myd first file written from Python
Hello world!
Ciao, my first file written from Python
Hello world!
Ciao, my first file written from Python
Hello world!
```

OUT:
```
f1: Hello world!
f2: Hello worlda!
f1: Ciao, my first file written from Python
f2: Ciao, myd first file written from Python
f2 only: Ciao, my first file written from Python
f2 only: Hello world!
```

## Solution

```py
def diff(f1, f2):
    with open(f1) as first:
        with open(f2) as second:
            while True:
                l1 = first.readline()
                l2 = second.readline()
                if not l1 and not l2:
                    break
                elif not l1:
                    print(f'f2 only: {l2}', end='')
                elif not l2:
                    print(f'f1 only: {l1}', end='')
                elif l1 != l2:
                    print(f'f1: {l1}', end='')
                    print(f'f2: {l2}', end='')
```
