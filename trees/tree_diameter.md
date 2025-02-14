# Diametro di un albero

Il diametro di un albero (non per forza binario) corrisponde al numero di nodi presenti sul livello "più grande", ovvero che contiene più nodi. Ad esempio, il diametro del seguente albero è 5, corrispondente al terzo livello:

![t1](https://images.javatpoint.com/ds/images/tree.png)

Scrivere una funzione `diameter(tree)` che dato un albero generico `tree` (definito tramite la classe `Tree` già presente nella soluzione dell'esercizio) ne ritorna il suo diametro. La funzione deve essere implementata come una **BFS** che calcola il diametro di ogni livello, per poi ritornarne il massimo.

## Template

```py
class Tree:
  def __init__(self, data):
    self.data = data
    self.children = []

  def add_node(self, father, child):
    if self == father:
      self.children.append(child)
    else:
      for c in self.children:
        c.add_node(father, child)

  def get_children(self):
    return self.children

  def is_leaf(self):
    return not self.children

  def get_data(self):
    return self.data

  def set_data(self, data):
    self.data = data

  def __str__(self):
    return self.str_aux('')

  def str_aux(self, indent):
    if self.is_leaf():
      return str(self.get_data())
    padding = indent.replace('c', '|  ').replace('l', '   ')
    ch_str = ''
    for c in self.get_children():
      ch_str += padding + '+- ' + c.str_aux(indent + ('c' if self.get_children().index(c) != len(self.get_children()) - 1 else 'l')) + '\n'
    return (f'{self.get_data()}\n{ch_str}').strip()

if __name__ == '__main__':
  nnodes = int(input())
  adj = dict()
  nodes = dict()
  root = None
  tree = None

  for _ in range(nnodes):
    n = int(input())
    node = Tree(int(n))
    nodes[n] = node
    adj[n] = list()
    if root == None:
      root = n
      tree = node

  nedges = int(input())
  for _ in range(nedges):
    src, dest = input().split()
    adj[int(src)].append(int(dest))

  worklist = [root]
  while worklist:
    current = worklist.pop(0)
    node = nodes[current]
    for dest in adj[current]:
      tree.add_node(node, nodes[dest])
      worklist.append(dest)

  print('diameter:', diameter(tree))
```

## Testcases

### 1

IN:
```
7
1
2
3
4
5
6
7
6
1 2
1 3
2 4
2 5
3 6
3 7
```

OUT:
```
diameter: 4
```

### 2

IN:
```
1
1
0
```

OUT:
```
diameter: 1
```

### 3

IN:
```
4
1
2
3
4
3
1 2
2 3
3 4
```

OUT:
```
diameter: 1
```

### 4

IN:
```
15
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
14
1 2
1 3
1 4
2 5
3 6
3 7
4 8
4 9
4 10
8 11
8 12
8 13
8 14
9 15
```

OUT:
```
diameter: 6
```

## Solution

```py
def diameter(tree):
  worklist = [(1, tree)]
  diameters = []
  previous_level = 1
  current_diameter = 0
  while worklist:
    level, current = worklist.pop(0)
    if level != previous_level:
      previous_level = level
      diameters.append(current_diameter)
      current_diameter = 0
    current_diameter += 1
    for child in current.get_children():
      worklist.append((level + 1, child))
  diameters.append(current_diameter)

  return max(diameters)
```
