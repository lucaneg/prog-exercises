# Numerazione linee

Scrivere una funzione `numbering(fname)` che dato il nome di un file `fname` legge il suo contenuto e aggiunge a ogni riga il suo numero all'inizio (alla prima riga viene aggiunto `'1: '`, alla seconda `'2: ', ...). Il contenuto del file va sostituito con queste nuove righe.

## Template

```py
if __name__ == '__main__':
    nlines = int(input())
    with open('/execution/test.txt', 'w') as f:
        for _ in range(nlines):
            f.write(input() + '\n')
    print(numbering('/execution/test.txt'))
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
```

OUT:
```
1: Ciao, my first file written from Python
2: Hello world!
3: Ciao, my first file written from Python
4: Hello world!
5: Ciao, my first file written from Python
6: Hello world!
7: Ciao, my first file written from Python
8: Hello world!
9: Ciao, my first file written from Python
10: Hello world!
```

## Solution

```py
def numbering(fname):
    with open(fname) as f:
        lines = f.readlines()
    for i in range(len(lines)):
        lines[i] = f'{i+1}: ' + lines[i]
    with open(fname, 'w') as f:
        for line in lines:
            f.write(line)
```
