# Alberi binari simmetrici

Un albero binario è *simmetrico* se scambiando tutti i figli di destra con quelli di sinistra in ogni nodo si ottiene lo stesso albero.

E' possibile controllare se un albero è simmetrico ricorsivamente, utilizzando il concetto di specularità tra due alberi. Due alberi `A` e `B` sono speculari se:

* la radice di `A` e la radice di `B` hanno lo stesso valore
* il **figlio destro** di `A` è speculare a quello **di sinistra** di `B`
* il **figlio sinistro** di `A` è speculare a quello **di destra** di `B`

Un albero è simmetrico se il figlio destro e il figlio sinistro della radice sono speculari.

Scrivere una funzione `is_symmetric(tree)` che, dato un albero binario `tree` ricevuto come parametro, ritorna `True` se l'albero è simmetrico. Per farlo dovrà usare una funzione `is_mirror(first, second)` che controlla se `first` e `second` sono speculari secondo la definizione riportata sopra. `is_mirror` deve essere implementato ricorsivamente come una DFS.

## Template

```py
class BinaryTree:
  def __init__(self, data):
    self.data = data
    self.left = None
    self.right = None

  def set_left_child(self, father, child):
    if self == father:
      self.left = child
    else:
      if self.left:
        self.left.set_left_child(father, child)
      if self.right:
        self.right.set_left_child(father, child)

  def set_right_child(self, father, child):
    if self == father:
      self.right = child
    else:
      if self.left:
        self.left.set_right_child(father, child)
      if self.right:
        self.right.set_right_child(father, child)

  def get_left_child(self):
    return self.left

  def get_right_child(self):
    return self.right

  def is_leaf(self):
    return self.left is None and self.right is None

  def get_data(self):
    return self.data

  def set_data(self, data):
    self.data = data

if __name__ == "__main__":
  nnodes = int(input())
  adj = dict()
  nodes = dict()
  root = None
  tree = None

  for _ in range(nnodes):
    idx, n = input().split(':')
    node = BinaryTree(int(n))
    nodes[idx] = node
    adj[idx] = list()
    if root == None:
      root = idx
      tree = node

  nedges = int(input())
  for _ in range(nedges):
    src, side, dest = input().split()
    adj[src].append((side, dest))

  worklist = [root]
  while worklist:
    current = worklist.pop(0)
    node = nodes[current]
    for side, dest in adj[current]:
      if side == 'l':
        tree.set_left_child(node, nodes[dest])
      else:
        tree.set_right_child(node, nodes[dest])
      worklist.append(dest)

  print(is_symmetric(tree))
```

## Testcases

### 1

IN:
```
7
1:7
2:4
3:15
4:9
5:6
6:2
7:3
6
1 l 2
1 r 3
2 l 4
2 r 5
3 l 6
3 r 7
```

OUT:
```
False
```

### 2

IN:
```
7
1:7
2:4
3:4
4:9
5:9
6:9
7:9
6
1 l 2
1 r 3
2 l 4
2 r 5
3 l 6
3 r 7
```

OUT:
```
True
```

### 3

IN:
```
7
1:7
2:4
3:4
4:9
5:8
6:8
7:9
6
1 l 2
1 r 3
2 l 4
3 l 5
2 r 6
3 r 7
```

OUT:
```
True
```

### 4

IN:
```
3
1:7
2:4
3:4
2
1 l 2
2 r 3
```

OUT:
```
False
```

### 5

IN:
```
7
1:7
2:4
3:4
4:9
5:8
6:8
7:9
6
1 l 2
1 r 3
2 l 4
3 l 5
3 r 6
2 r 7
```

OUT:
```
False
```

### 6

IN:
```
1
1:7
0
```

OUT:
```
True
```

## Solution

```py
def is_symmetric(tree):
  if not tree:
    return True

  def is_mirror(first, second):
    if not first and not second:
      return True
    if not first or not second:
      return False
    lr = is_mirror(first.get_left_child(), second.get_right_child())
    rl = is_mirror(first.get_right_child(), second.get_left_child())
    return first.get_data() == second.get_data() and lr and rl

  return is_mirror(tree.get_left_child(), tree.get_right_child())
```
