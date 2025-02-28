# Cards and Decks

There are fifty-two cards in a deck, each of which belongs to one of four suits and one of thirteen ranks. The suits are Spades, Hearts, Diamonds, and Clubs. The ranks are Ace, 2, 3, 4, 5, 6, 7, 8, 9, 10, Jack, Queen, and King. Depending on the game that you are playing, an Ace may be higher than King or lower than 2. Definie the class for a generic card. We will use integers to represent both suits and ranks, and we will map those integers to the corresponding suit/rank. The `None` in the rank names is just a placeholder to have the ace sitting at position 1, so we can use the card number as its rank.

We then make a deck, that is composed by a list of cards. We also add methods for filling the deck with all the cards, shuffle it, draw a card from the top, and adding a card.

Then we create a hand, that is a list of cards held by a player. A hand can be seen as a special instance of a deck with fewer cards: we will thus use inheritance to implement it.

Finally, we simulate two players drawing cards to play poker.

## Solution

```py
class Card:
  suit_names = ['spades', 'hearts', 'diamonds', 'clubs']
  rank_names = [None, 'ace', '2', '3', '4', '5', '6', '7', '8', '9', '10', 'jack', 'queen', 'king']

  def __init__(self, suit, rank):
    self.suit = suit
    self.rank = rank

  def __str__(self):
    return f'{Card.rank_names[self.rank]} of {Card.suit_names[self.suit]}'

queen_of_spades = Card(0, 12)
print(queen_of_spades)

class Deck:
  def __init__(self):
    self.cards = []

  def __str__(self):
    return '\n'.join([str(c) for c in self.cards])

  def draw(self):
    return self.cards.pop()

  def add_card(self, card):
    self.cards.append(card)

  def fill(self):
    for s in range(len(Card.suit_names)):
      for r in range(1, len(Card.rank_names)):
        self.add_card(Card(s, r))

  def shuffle(self):
    import random
    random.shuffle(self.cards)

deck = Deck()
deck.fill()
deck.shuffle()
print(deck)

class Hand(Deck):
  def __init__(self, owner):
    super().__init__()
    self.owner = owner

  def __str__(self):
    return f'Hand of {self.owner}:\n{super().__str__()}'

p1 = Hand('player 1')
p2 = Hand('player 2')

p1.add_card(deck.draw())
p1.add_card(deck.draw())

p2.add_card(deck.draw())
p2.add_card(deck.draw())

table = Hand('table')
table.add_card(deck.draw())
table.add_card(deck.draw())
table.add_card(deck.draw())
table.add_card(deck.draw())
table.add_card(deck.draw())

print(table)
print()
print(p1)
print()
print(p2)

print()
print(deck)
```
