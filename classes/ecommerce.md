# E-commerce

Un sito e-commerce (cioè un negozio online) vende un insieme di prodotti. Un prodotto generico è descritto dalla classe `Product` che contiene gli attributi nome, prezzo, e disponibilità (un numero che rappresenta quanti pezzi sono disponibili in magazzino). Tutti gli attributi vengono creati nel costruttore utilizzando parametri. I prodotti possono essere di due tipi:
- prodotti fisici, descritti dalla classe `PhysicalProduct` che eredita da Product, che contiene un attributo aggiuntivo per i costi di spedizione (inizializzato anche questo nel costruttore usando un parametro)
- prodotti digitali, descritti dalla classe `DigitalProduct` che eredita da Product, che non contiene attributi aggiuntivi ma imposta sempre la disponibilità a 1 **senza usare un parametro**

Infine, gli utenti del sito possono effettuare ordini, definiti dalla classe `Order`, che contengono una lista di prodotti che viene sempre creata vuota nel costruttore.

`Product`, `DigitalProduct` e `PhysicalProduct` **non devono contenere metodi**. `Order` invece deve contenere:
- l'operatore `+=` per aggiungere prodotti alla lista di quelli ordinati, che deve lanciare un `TypeError` se i parametri non sono dei tipi corretti e un `ValueError` se il prodotto aggiunto non è più disponibile; 
- l'operatore necessario per convertire un ordine in stringa (la stringa ritornata deve contenere un prodotto per riga, e il singolo prodotto deve essere visualizzato come `<nome> (<prezzo>)`)
- un metodo `get_total` che ritorna il costo totale dell'ordine, sommando i prezzi di tutti i prodotti e aggiungendo le spese di spedizione per i soli prodotti fisici

Tutti i metodi e gli operatori che hanno bisogno di distinguere in base al tipo di prodotto devono utilizzare il **type-based dispatching** per distinguere tra prodotti fisici e digitali, comportandosi di conseguenza. L'operatore `+=` deve decrementare la disponibilità del prodotto aggiunto all'ordine se il prodotto è fisico.

## Template

```py
if __name__ == '__main__':
  n_prod = input()
  products = []
  for _ in range(int(n_prod)):
    c = input().split()
    if c[0] == 'p':
      products.append(PhysicalProduct(c[1], float(c[2]), int(c[3]), float(c[4])))
    else:
      products.append(DigitalProduct(c[1], float(c[2])))
  
  ord = input().split()
  order = Order()
  for p in ord:
    try:
      order += products[int(p)]
    except ValueError:
      print(f'Error on adding {products[int(p)].name} to order')
  print(order)
  
  print(order.get_total())
```

## Testcases

### 1

IN:
```
3
p Cassa 68.2 3 13.5
p Padella 23.65 1 5.50
d EBook 32.15
0 0 1 2
```

OUT:
```
Cassa (68.2)
Cassa (68.2)
Padella (23.65)
EBook (32.15)
224.70000000000002
```

### 2

IN:
```
3
p Cassa 68.2 3 13.5
p Padella 23.65 1 5.50
d EBook 32.15
0 0 1 1 2
```

OUT:
```
Error on adding Padella to order
Cassa (68.2)
Cassa (68.2)
Padella (23.65)
EBook (32.15)
224.70000000000002
```

### 3

IN:
```
3
p Cassa 68.2 3 13.5
p Padella 23.65 1 5.50
d EBook 32.15
0 0 1 2 2 2 2
```

OUT:
```
Cassa (68.2)
Cassa (68.2)
Padella (23.65)
EBook (32.15)
EBook (32.15)
EBook (32.15)
EBook (32.15)
321.15
```

## Solution

```py
class Product:
  def __init__(self, name, price, availability):
    self.name = name
    self.price = price
    self.availability = availability

class PhysicalProduct(Product):
  def __init__(self, name, price, availability, shipping_costs):
    super().__init__(name, price, availability)
    self.shipping_costs = shipping_costs

class DigitalProduct(Product):
  def __init__(self, name, price):
    super().__init__(name, price, 1)

class Order:
  def __init__(self):
    self.products = []
  
  def __iadd__(self, other):
    if not isinstance(other, Product):
      raise TypeError()
    if other.availability < 1:
      raise ValueError()
    self.products.append(other)
    if not isinstance(other, DigitalProduct):
      other.availability -= 1
    return self

  def __str__(self):
    prods = [f'{p.name} ({p.price})' for p in self.products]
    return '\n'.join(prods)
  
  def get_total(self):
    total = 0
    for prod in self.products:
      total += prod.price
      if isinstance(prod, PhysicalProduct):
        total += prod.shipping_costs
    return total
```
