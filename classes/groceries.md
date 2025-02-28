# Groceries shopping

Write a class for a food item that you can place in your shopping cart. Such an item will have a weight and a price per kilogram, and will provide functionalities to calculate the exact price for that item. Then add few types of food that we might buy, each one having a fixed price per kilogram. Finally, create a shopping cart that allows us to add items, print the contents and calculate the total price.


## Solution

```py
class FoodItem:
  def __init__(self, weight, price_per_kg):
    self.weight = weight
    self.price_per_kg = price_per_kg

  def get_price(self):
    return self.weight * self.price_per_kg

  def __str__(self):
    return f'{self.weight} kg of {self.get_name()} (total price: {self.get_price()})'

class Apples(FoodItem):
  def __init__(self, weight):
    super().__init__(weight, 1.15)

  def get_name(self):
    return 'apples'

class Meat(FoodItem):
  def __init__(self, weight):
    super().__init__(weight, 29.71)

  def get_name(self):
    return 'meat'

class Bread(FoodItem):
  def __init__(self, weight):
    super().__init__(weight, 0.48)

  def get_name(self):
    return 'bread'

class ShoppingCart:
  def __init__(self):
    self.items = []

  def __iadd__(self, item):
    self.items.append(item)
    return self

  def __str__(self):
    return '\n'.join([str(i) for i in self.items])

  def get_total_price(self):
    return sum([i.get_price() for i in self.items])

cart = ShoppingCart()
cart += Apples(2.2)
cart += Bread(0.6)
cart += Meat(0.8)
cart += Meat(1.7)

print(cart)
print(cart.get_total_price())
```
