# Asta di opere d'arte

In un asta di opere d'arte si vendono due tipi di opere: quadri e sculture.
I prodotti vengono rappresentati tramite classi: `Painting` per i quadri e `Sculpture` per le sculture. Entrambe le classi ereditano dalla classe `Artwork`, che definisce gli attributi comuni a entrambe: titolo, artista e prezzo. Le due sottoclassi si differenziano per degli attributi aggiuntivi:
- `Painting` contiene anche il tipo di pittura (a olio, tempere, acquerelli, ...)
- `Sculpture` contiene il materiale utilizzato (bronzo, marmo, ...)

Tutti gli attributi vanno creati nel costruttore e inizializzati usando dei parametri. `Artwork` deve contenere l'operatore `>=` (`__ge__`) che confronta l'opera con un numero float, ritornando `True` se il prezzo dell'opera è maggiore del numero dato (va lanciato un `TypeError` se i tipi dei parametri non sono corretti). `Artwork` deve anche definire l'operatore per la conversione in stringa, che ritorna `<titolo> by <autore>, price: <prezzo>`. Le due sottoclassi devono poi fare l'override di tale operatore, aggiungendo alla stringa ritornata dal padre `, painting type: <tipo di pittura>` per `Painting`, e `, material: <materiale>` per `Sculpture`.

L'asta è rappresentata dalla classe `Auction`, che contiene una lista di opere in vendita (la lista viene creata vuota dal costruttore). Deve essere possibile aggiungere opere tramite l'operatore `+=`, che lancia un `TypeError` se i parametri non sono del tipo corretto. La classe deve anche contenere l'operatore necessario per la conversione in stringa, che ritorna le opere su linee diverse (separate da `\n`). Infine, `Auction` deve contenere un metodo `get_all_by_feature(name)` che ritorna una lista contenente tutte le opere che (i) sono quadri e hanno come tipo di pittura `name`, oppure che (ii) sono sculture e hanno come materiale `name`. **Nota: non va usato il type-based dispatching per capire il tipo dell'opera.** Usare invece il duck-typing per controllare se l'opera contiene l'attributo pittura/materiale, e comportarsi di conseguenza.

## Template

```py
if __name__ == '__main__':
  n_art = input()
  
  artworks = []
  for _ in range(int(n_art)):
    c = input().split()
    if c[0] == 'p':
      artworks.append(Painting(c[1], c[2], float(c[3]), c[4]))
    else:
      artworks.append(Sculpture(c[1], c[2], float(c[3]), c[4]))
  
  auction = Auction()
  for a in artworks:
    auction += a
  print(auction)

  n_op = int(input())
  for _ in range(int(n_art)):
    o = input().split()
    if o[0] == 'c':
      print(artworks[int(o[1])] >= float(o[2]))
    else:
      print(', '.join([str(a) for a in auction.get_all_by_feature(o[1])]))
```

## Testcases

### 1

IN:
```
3
p Painting1 Autore1 1234.56 oil
p Painting2 Autore2 655.50 oil
s Sculpture1 Autore1 3786.12 bronze
3
c 0 500
c 1 1000
g oil
```

OUT:
```
Painting1 by Autore1, price: 1234.56, painting type: oil
Painting2 by Autore2, price: 655.5, painting type: oil
Sculpture1 by Autore1, price: 3786.12, material: bronze
True
False
Painting1 by Autore1, price: 1234.56, painting type: oil, Painting2 by Autore2, price: 655.5, painting type: oil
```

### 2

IN:
```
3
p Painting1 Autore1 1234.56 oil
p Painting2 Autore2 655.50 oil
s Sculpture1 Autore1 3786.12 bronze
3
c 0 500
c 1 1000
g bronze
```

OUT:
```
Painting1 by Autore1, price: 1234.56, painting type: oil
Painting2 by Autore2, price: 655.5, painting type: oil
Sculpture1 by Autore1, price: 3786.12, material: bronze
True
False
Sculpture1 by Autore1, price: 3786.12, material: bronze
```

### 3

IN:
```
3
p Painting1 Autore1 1234.56 oil
p Painting2 Autore2 655.50 oil
s Sculpture1 Autore1 3786.12 bronze
3
c 0 500
c 1 1000
g marble
```

OUT:
```
Painting1 by Autore1, price: 1234.56, painting type: oil
Painting2 by Autore2, price: 655.5, painting type: oil
Sculpture1 by Autore1, price: 3786.12, material: bronze
True
False
```

## Solution

```py
class Artwork:
  def __init__(self, title, author, price):
    self.title = title
    self.author = author
    self.price = price
  
  def __str__(self):
    return f'{self.title} by {self.author}, price: {self.price}'
  
  def __ge__(self, other):
    if not isinstance(other, float):
      raise TypeError()
    return self.price >= other

class Painting(Artwork):
  def __init__(self, title, author, price, paint):
    super().__init__(title, author, price)
    self.paint = paint
  
  def __str__(self):
    return super().__str__() + f', painting type: {self.paint}'

class Sculpture(Artwork):
  def __init__(self, title, author, price, material):
    super().__init__(title, author, price)
    self.material = material
  
  def __str__(self):
    return super().__str__() + f', material: {self.material}'

class Auction:
  def __init__(self):
    self.artworks = []
  
  def __iadd__(self, other):
    if not isinstance(other, Artwork):
      raise TypeError()
    self.artworks.append(other)
    return self
  
  def __str__(self):
    works = [str(a) for a in self.artworks]
    return '\n'.join(works)
  
  def get_all_by_feature(self, name):
    res = []
    for art in self.artworks:
      if hasattr(art, 'paint') and art.paint == name:
        res.append(art)
      elif hasattr(art, 'material') and art.material == name:
        res.append(art)
    return res
```
