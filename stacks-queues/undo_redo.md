# Annulla e ripeti

Si vuole simulare un editor di testo con le funzioni annulla e ripeti attraverso la funzione `editor(commands)`. Tale funzione riceve una lista di stringhe che corrispondono ai comandi ricevuti dall'utente. Commands può contenere:

* stringhe generiche che rappresentano i caratteri inseriti dall'utente
* la stringa speciale `#undo#` che annulla l'ultimo carattere inserito
* la stringa speciale `#redo#` che ri-aggiuge l'ultimo elemento eliminato con `#undo#`

`editor` deve costruire una lista contenete il risultato delle operazioni svolte dall'utente. Al termine della funzione, tale lista deve essere trasformata in una stringa senza aggiungere spazi (`''.join(result)`). La lista risultato deve essere costruita aggiungendo tutte le stringhe generice così come ricevute, e deve mantenere uno **stack** di comandi annullati attraverso `#undo#`. Quando viene ricevuto il comando `#undo#`, l'ultima stringa aggiunta al risultato va rimossa, e va aggiunta allo stack. Se si riceve `#redo#`, l'elemento in cima allo stack va rimosso ed aggiunto al risultato.

**Aggiungere una stringa generica al risultato causa lo svuotamento dello stack, e rende impossibile utilizzare il comando** `#redo#`**.** Se `#redo#` o viene utilizzato a stack vuoto, o se `#undo#` viene utilizzato quando la lista risultato è vuota, la funzione deve lanciare una `OperationNotAvailableException`.

## Template

```py
class OperationNotAvailableException(Exception):
  pass

if __name__ == '__main__':
  n = int(input())
  commands = []
  for _ in range(n):
    commands.append(input())
  try:
    print(editor(commands))
  except OperationNotAvailableException:
    print('operation not available')
```

## Testcases

### 1

IN:
```
4
a
b
c
d
```

OUT:
```
abcd
```

### 2

IN:
```
12
Hello
 world
#undo#
!
#undo#
#undo#
#redo#
 there
#undo#
#redo#
#redo#
!!!
```

OUT:
```
operation not available
```

### 3

IN:
```
14
Alice
 and
 Bob
 are friends
#undo#
 love coding
#undo#
#redo#
 and enjoy hiking
#undo#
#redo#
#undo#
#redo#
 together
```

OUT:
```
Alice and Bob love coding and enjoy hiking together
```

### 4

IN:
```
11
Hello
 world
#undo#
!
#undo#
#undo#
#redo#
 there
#undo#
#redo#
!!!
```

OUT:
```
Hello there!!!
```

### 5

IN:
```
15
Alice
 and
 Bob
 are friends
#undo#
 love coding
#undo#
#redo#
 and enjoy hiking
#undo#
#redo#
#redo#
#undo#
#redo#
 together
```

OUT:
```
operation not available
```

### 6

IN:
```
18
Alice
 and
 Bob
#undo#
#undo#
#undo#
#undo#
 are friends
#undo#
 love coding
#undo#
#redo#
 and enjoy hiking
#undo#
#redo#
#undo#
#redo#
 together
```

OUT:
```
operation not available
```

## Solution

```py
def editor(commands):
  result = []
  stack = []
  for c in commands:
    if c == '#undo#':
      if not result:
        raise OperationNotAvailableException()
      stack.append(result.pop())
    elif c == '#redo#':
      if not stack:
        raise OperationNotAvailableException()
      result.append(stack.pop())
    else:
      stack.clear()
      result.append(c)
  return ''.join(result)
```
