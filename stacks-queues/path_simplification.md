# Semplificazione di percorsi

I file presenti sull'hard disk di un computer sono identificati da dei percorsi assoluti: `/un/percorso/con/diverse/cartelle/file.txt`. Questi percorsi iniziano sempre con uno `/`, per poi contenere una serie di nomi di cartelle separati da `/`. L'ultimo nome riportato è quello del file a cui si fa riferimento.

E' anche possibile costruire dei percorsi relativi, utilizzando i caratteri speciali `.` (che rappresenta la cartella corrente) e `..` (che rappresenta la cartella precedente nel percorso. I seguenti percorsi sono quindi equivalenti al precedente:
- `/un/percorso/./con/diverse/cartelle/file.txt`
- `/un/percorso/con/../con/diverse/cartelle/file.txt`
- `/un/percorso/./../percorso/con/diverse/cartelle/../cartelle/file.txt`

Scrivere una funzione `simplify_path(path)` che, data una stringa `path` rappresentante un percorso relativo, ricostruisce il percorso assoluto corrispondente utilizzando uno stack. La stringa va suddivisa in 'token' ogni volta che compare un `/`, che vanno processati nel seguente modo:
- il token `.` va ignorato
- il token `..` scarta l'ultimo token processato
- tutti gli altri token vanno aggiunti al risultato

I token processati vanno aggiunti a uno stack che viene manipolato dalla funzione secondo le regole date. Al termine, la funzione `simplify_path` deve utilizzare il contenuto dello stack per ricostruire il percorso assoluto, inserendo i `/` tra i vari token.

## Template

```py
if __name__ == '__main__':
  print(simplify_path(input()))
```

## Testcases

### 1

IN:
```
/un/percorso/con/diverse/cartelle/file.txt
```

OUT:
```
/un/percorso/con/diverse/cartelle/file.txt
```

### 2

IN:
```
/un/percorso/con/../con/diverse/cartelle/file.txt
```

OUT:
```
/un/percorso/con/diverse/cartelle/file.txt
```

### 3

IN:
```
/un/percorso/./con/diverse/cartelle/file.txt
```

OUT:
```
/un/percorso/con/diverse/cartelle/file.txt
```

### 4

IN:
```
/un/percorso/./../percorso/con/diverse/cartelle/../cartelle/file.txt
```

OUT:
```
/un/percorso/con/diverse/cartelle/file.txt
```

### 5

IN:
```
/un/percorso/../con/.././../con/diverse/cartelle/../
```

OUT:
```
/con/diverse
```

## Solution

```py
def simplify_path(path):
  tokens = path.split('/')
  stack = []
  for token in tokens:
    if token == '.' or len(token) == 0:
      continue
    elif token == '..':
      stack.pop()
    else:
      stack.append(token)
  return '/' + '/'.join(stack)
```
