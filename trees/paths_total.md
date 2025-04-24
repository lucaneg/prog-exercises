# Somma di tutti i percorsi di un albero

Scrivere una funzione `sum_paths(tree)` che, dato un albero (non binario, definito ricorsivamente tramite la classe `Tree`) come parametro, ritorna la somma dei totali di percorsi dalla dalla a una delle foglie. Dato un singolo percorso, il suo totale corrisponde alla somma di tutti i valori presenti nei nodi attraversati. La funzione quindi calcolare il totale di ogni percorso e poi sommarli.

Ad esempio, dato l'albero:
```
5
+-- 9
|   +-- 1
|   +-- 3
|   |   +-- 4
|   +-- 2
+-- 7
    +-- 11
```

I suoi percorsi dalla radice alla foglia sono:
- 5, 9, 1, totale: 15
- 5, 9, 3, 4, totale: 21
- 5, 9, 2, totale: 16
- 5, 7, 11, totale: 23

`sum_paths` deve quindi ritornare 75.

Usare una DFS. Se necessario, è possibile aggiungere parametri aggiuntivi alla funzione, purché abbiano dei valori di default. Suggerimento: non è per forza necessario calcolare tutti i percorsi per poi sommarli. Si può anche calcolare la somma man mano che si attraversa l'albero. Per avere il punteggio massimo in questo esercizio bisogna risolverlo in questo modo, altrimenti il massimo è 8/10.

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

  print(sum_paths(tree))
```

## Testcases

### 1

IN:
```
8
5
9
1
3
4
2
7
11
7
5 9
5 7
7 11
9 1
9 3
3 4
9 2
```

OUT:
```
75
```

### 2

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
36
```

### 3

IN:
```
1
1
0
```

OUT:
```
1
```

### 4

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
10
```

### 5

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
175
```

## Solution

```py
def sum_paths(tree, current_sum=0):
  if not tree:
    return 0
  current_sum += tree.get_data()
  if len(tree.get_children()) == 0:
    return current_sum
  total = 0
  for child in tree.get_children():
    total += sum_paths(child, current_sum)
  return total
```
