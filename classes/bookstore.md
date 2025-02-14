# Libreria

Creare la classe `Book` con due attributi `title` e `author`, entrambi inizializzati tramite un costruttore. La classe deve implementare l'operatore `==`. Inoltre, la classe deve implementare il seguente operatore (che puo' essere copiato e incollato in quanto fuori dagli obiettivi del corso):
```python
  def __hash__(self):
    return hash(self.title) ^ hash(self.author)
```

Implementare poi una classe `BookStore` che contiene un unico attributo `books` che è sempre inizializzato a un dizionario vuoto (**senza ricevere un parametro nel costruttore**). L'attributo `books` conterrà tutti i libri disponibili nel negozio, e il numero di copie attualmente disponibili: se `books` contiene la chiave `Il Signore Degli Anelli` a cui corrisponde il valore `3` significa che ci sono attualmente 3 copie a disposizione per quel libro.

`BookStore` deve implementare i seguenti metodi/operatori:
- variante in-place dell'operatore `+`, che deve lanciare un `TypeError` se l'argomento ricevuto come parametro non è di tipo `Book`; se il tipo è corretto va aggiunta una copia disponibile per quel libro (nota: il libro potrebbe non essere abcora presente in `books`!)
- metodo `borrow_book` che riceve un parametro `book` e che rappresenta il prestito di un libro:
  - se il libro non è tra quelli conosciuti (cioè non è presente in `books`), il metodo deve lanciare una `UnknownBookException`
  - se il libro è conosciuto ma non è disponibile (numero di copie inferiore a 1), il metodo deve lanciare una `UnavailableBookException`
  - altrimenti, il metodo decrementa il numero di copie disponibili per il libro di 1
- `return_book` che riceve un parametro `book` e che rappresenta la restituzione di un libro:
  - se il libro non è tra quelli conosciuti (cioè non è presente in `books`), il metodo deve lanciare una `UnknownBookException`
  - altrimenti, il metodo incrementa il numero di copie disponibili per il libro di 1
- `available_copies` che riceve un parametro `book` e che restituisce il numero di copie disponibili di quel libro:
  - se il libro non è tra quelli conosciuti (cioè non è presente in `books`), il metodo deve lanciare una `UnknownBookException`
  - altrimenti, il metodo ritorna il numero di copie disponibili

## Template

```py
class UnknownBookException(Exception):
  pass

class UnavailableBookException(Exception):
  pass

if __name__ == '__main__':
  nbooks = int(input())
  store = BookStore()
  books = dict()
  for _ in range(nbooks):
    name, author, count = input().split()
    b = Book(name, author)
    books[name + author] = b
    for _ in range(int(count)):
      store += b
  
  nops = int(input())
  for _ in range(nops):
    op, name, author = input().split()
    try:
      if op == 'B':
        store.borrow_book(Book(name, author))
      else:
        store.return_book(Book(name, author))
    except UnknownBookException:
      print("UnknownBookException on " + name + " " + author)
      break
    except UnavailableBookException:
      print("UnavailableBookException on " + name + " " + author)
      break

  for b in books:
    print(store.available_copies(books[b]))
```

## Testcases

### 1

IN:
```
5
T1 A 2
T2 B 5
T3 C 1
T4 C 7
T5 A 4
3
B T1 A
R T1 A
B T3 C
```

OUT:
```
2
5
0
7
4
```

### 2

IN:
```
5
T1 A 2
T2 B 5
T3 C 1
T4 C 7
T5 A 4
3
B T1 A
R T7 A
B T3 C
```

OUT:
```
UnknownBookException on T7 A
1
5
1
7
4
```

### 3

IN:
```
5
T1 A 2
T2 B 5
T3 C 1
T4 C 7
T5 A 4
2
B T3 C
B T3 C
```

OUT:
```
UnavailableBookException on T3 C
2
5
0
7
4
```

### 4

IN:
```
5
T1 A 2
T2 B 5
T3 C 1
T4 C 7
T5 A 4
3
B T7 A
R T1 A
B T3 C
```

OUT:
```
UnknownBookException on T7 A
2
5
1
7
4
```

## Solution

```py
class Book:
  def __init__(self, title, author):
    self.title = title
    self.author = author

  def __eq__(self, other):
    if not isinstance(other, Book):
      return False
    return self.title == other.title and self.author == other.author

  def __hash__(self):
    return hash(self.title) ^ hash(self.author)

class BookStore:
  def __init__(self):
    self.books = dict()

  def __iadd__(self, book):
    if not isinstance(book, Book):
      raise TypeError
    elif book in self.books:
      self.books[book] = self.books[book] + 1
    else:
      self.books[book] = 1
    return self

  def borrow_book(self, book):
    if book not in self.books:
      raise UnknownBookException
    elif self.books[book] < 1:
      raise UnavailableBookException
    else:
      self.books[book] = self.books[book] - 1

  def return_book(self, book):
    if book in self.books:
      self.books[book] = self.books[book] + 1
    else:
      raise UnknownBookException

  def available_copies(self, book):
    if book in self.books:
      return self.books[book] 
    else:
      raise UnknownBookException
```
