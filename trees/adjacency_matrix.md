# Matrice di adiacenza di un albero

Scrivere una funzione `adjacency_matrix(tree, n_nodes)` che, dato un albero non-binario `tree` definito ricorsivamente e i cui nodi sono numerati da 0 a `n_nodes-1`, ritorni una matrice (non di NumPy, ma una lista di liste) di dimensione `n_nodes x n_nodes` che rappresenta la matrice di adiacenza dell'albero. La matrice deve contenere in posizione `i,j`:
- 0 se non esiste né un arco dal nodo `i` al nodo `j` né un arco dal nodo `j` al nodo `i`
- 1 se esiste un arco dal nodo `i` al nodo `j`
- -1 se esiste un arco dal nodo `j` al nodo `i`

La funzione deve procedere creando una matrice di dimensioni appropriate, per poi riempirla con una visita a piacere. E' possibile utilizzare sia una BFS che una DFS (ricorsiva o non) a piacimento.

Link utili:
- [Documentazione Python](https://docs.python.org/3/)
- [Google Colab](https://colab.google.com)
- [Moodle](https://moodle.unive.it)

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

  res = adjacency_matrix(tree, nnodes)
  print('    |', ' | '.join(f'{e:>#2}' for e in range(nnodes)))
  print('+'.join('----' for _ in range(nnodes+1)))
  for i in range(nnodes):
    print(f' {i:>#2} |', ' | '.join(f'{e:>#2}' for e in res[i]))
```

## Testcases

### 1

IN:
```
8
0
1
2
3
4
5
6
7
7
0 1
0 6
6 7
1 2
1 3
3 4
1 5
```

OUT:
```
    |  0 |  1 |  2 |  3 |  4 |  5 |  6 |  7
----+----+----+----+----+----+----+----+----
  0 |  0 |  1 |  0 |  0 |  0 |  0 |  1 |  0
  1 | -1 |  0 |  1 |  1 |  0 |  1 |  0 |  0
  2 |  0 | -1 |  0 |  0 |  0 |  0 |  0 |  0
  3 |  0 | -1 |  0 |  0 |  1 |  0 |  0 |  0
  4 |  0 |  0 |  0 | -1 |  0 |  0 |  0 |  0
  5 |  0 | -1 |  0 |  0 |  0 |  0 |  0 |  0
  6 | -1 |  0 |  0 |  0 |  0 |  0 |  0 |  1
  7 |  0 |  0 |  0 |  0 |  0 |  0 | -1 |  0
```

### 2

IN:
```
7
0
1
2
3
4
5
6
6
0 1
0 2
1 3
1 4
2 5
3 6
```

OUT:
```
    |  0 |  1 |  2 |  3 |  4 |  5 |  6
----+----+----+----+----+----+----+----
  0 |  0 |  1 |  1 |  0 |  0 |  0 |  0
  1 | -1 |  0 |  0 |  1 |  1 |  0 |  0
  2 | -1 |  0 |  0 |  0 |  0 |  1 |  0
  3 |  0 | -1 |  0 |  0 |  0 |  0 |  1
  4 |  0 | -1 |  0 |  0 |  0 |  0 |  0
  5 |  0 |  0 | -1 |  0 |  0 |  0 |  0
  6 |  0 |  0 |  0 | -1 |  0 |  0 |  0
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
    |  0
----+----
  0 |  0
```

### 4

IN:
```
4
0
1
2
3
3
0 1
1 2
2 3
```

OUT:
```
    |  0 |  1 |  2 |  3
----+----+----+----+----
  0 |  0 |  1 |  0 |  0
  1 | -1 |  0 |  1 |  0
  2 |  0 | -1 |  0 |  1
  3 |  0 |  0 | -1 |  0
```

### 5

IN:
```
15
0
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
14
0 1
0 2
0 3
1 4
2 5
2 6
3 7
3 8
3 9
7 10
7 11
7 12
7 13
8 14
```

OUT:
```
    |  0 |  1 |  2 |  3 |  4 |  5 |  6 |  7 |  8 |  9 | 10 | 11 | 12 | 13 | 14
----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----
  0 |  0 |  1 |  1 |  1 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0
  1 | -1 |  0 |  0 |  0 |  1 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0
  2 | -1 |  0 |  0 |  0 |  0 |  1 |  1 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0
  3 | -1 |  0 |  0 |  0 |  0 |  0 |  0 |  1 |  1 |  1 |  0 |  0 |  0 |  0 |  0
  4 |  0 | -1 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0
  5 |  0 |  0 | -1 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0
  6 |  0 |  0 | -1 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0
  7 |  0 |  0 |  0 | -1 |  0 |  0 |  0 |  0 |  0 |  0 |  1 |  1 |  1 |  1 |  0
  8 |  0 |  0 |  0 | -1 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  1
  9 |  0 |  0 |  0 | -1 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0
 10 |  0 |  0 |  0 |  0 |  0 |  0 |  0 | -1 |  0 |  0 |  0 |  0 |  0 |  0 |  0
 11 |  0 |  0 |  0 |  0 |  0 |  0 |  0 | -1 |  0 |  0 |  0 |  0 |  0 |  0 |  0
 12 |  0 |  0 |  0 |  0 |  0 |  0 |  0 | -1 |  0 |  0 |  0 |  0 |  0 |  0 |  0
 13 |  0 |  0 |  0 |  0 |  0 |  0 |  0 | -1 |  0 |  0 |  0 |  0 |  0 |  0 |  0
 14 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 | -1 |  0 |  0 |  0 |  0 |  0 |  0
```

## Solution

```py
def adjacency_matrix(tree, n_nodes):
  matrix = [[0] * n_nodes for _ in range(n_nodes)]

  def dfs(node):
    for child in node.get_children():
      matrix[node.get_data()][child.get_data()] = 1
      matrix[child.get_data()][node.get_data()] = -1
      dfs(child)

  dfs(tree)

  return matrix
```
