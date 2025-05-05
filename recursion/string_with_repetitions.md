# Stringhe con ripetizioni

Una stringa può contenere delle ripetizioni specificate da un numero (tra 1 e 9 inclusi) seguito da dei caratteri circondati da parentesi quadre, ad esempio `'ab2[cd]'`. Tale stringa va interpretata come *la stringa 'ab' seguita dalla stringa 'cd' ripetuta due volte*, e corrisponde quindi alla stringa `'abcdcd'`. Si noti che:
- le stringhe possono anche non contenere ripetizioni (ad esempio, `'abc'`);
- le ripetizioni possono essere innestate (ad esempio, `'ab2[c3[d]]'` che corrisponde a `'abcdddcddd'`).

Scrivere una funzione **ricorsiva** `decode(s)` che, data una stringa che contiene ripetizioni, ritorna la stringa decodificata in cui le ripetizioni sono espanse. La funzione deve quindi scorrere la stringa `s` e, ogni volta che trova un numero, deve (i) decodificare la stringa contenuta tra le parentesi quadre, e (ii) ripetere la stringa decodificata il numero necessario di volte. 

## Template

```py
if __name__ == '__main__':
  print(decode(input()))
```

## Testcases

### 1

IN:
```
abcd
```

OUT:
```
abcd
```

### 2

IN:
```
ab2[cd]
```

OUT:
```
abcdcd
```

### 3

IN:
```
ab2[c3[d]]
```

OUT:
```
abcdddcddd
```

### 4

IN:
```
a5[2[bc7[d]]]
```

OUT:
```
abcdddddddbcdddddddbcdddddddbcdddddddbcdddddddbcdddddddbcdddddddbcdddddddbcdddddddbcddddddd
```

## Solution

```py
def decode(s, start=0):
  i = start
  result = ''
  while i < len(s):
    if s[i].isdigit():
      n = int(s[i])
      i, nested = decode(s, i + 2)
      result += nested * n
    elif s[i] == ']':
      return i, result
    else:
      result += s[i]
    i += 1
  return result
```
