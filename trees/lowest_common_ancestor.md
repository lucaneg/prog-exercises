# Lowest Common Ancestor

In un albero (non per forza binario), i *discendenti* di un nodo sono tutti i nodi raggiungibili seguendo gli archi in uscita dal nodo stesso, incluso il nodo di partenza. Se l'albero è definito ricorsivamente, anche la definizione di discenti diventa ricorsiva: i discendenti di un nodo sono il nodo stesso più i discendenti di tutti i sottoalberi figli di quel nodo. Ad esempio, dato il seguente albero:

![image.png](https://raw.githubusercontent.com/lucaneg/inf1mod2/master/tree1.png)

- i discendenti del nodo 2 sono i nodi 2, 5, 6, 11;
- i discendenti del nodo 4 sono i nodi 4, 8, 9, 10;
- il discendente del nodo 7 è solamente il nodo 7.

Dati due nodi in un albero (non per forza binario), il loro *Lowest Common Ancestor* (LCA, parente comune più in basso) è il nodo più in profondità (cioè più in basso) all'interno dell'albero che ha come discendenti entrambi i nodi di partenza. Ad esempio, nell'albero riportato sopra:

- l'LCA dei nodi 5 e 6 è il nodo 2
- l'LCA dei nodi 7 e 4 è il nodo 1
- l'LCA dei nodi 4 e 9 è il nodo 4
- l'LCA dei nodi 11 e 11 è il nodo 11

Scrivere due funzioni, entrambe **DFS**:

- `descendants(tree)` che, dato un albero ricorsivo `tree`, ritorna l'insieme dei discendenti della radice dell'albero (suggerimento: nella lista di discendenti, va inserito il *dato* coneunto nel nodo, non il nodo stesso);
- `least_common_ancestor(tree, n1, n2)` che, dato un albero ricorsivo `tree` e due suoi nodi `n1` e `n2`, ritorna l'LCA di `n1` e `n2`. Notare che `n1`, `n2`, e il valore ritornato da `least_common_ancestor` sono **i dati contenuti nei nodi**, e non i nodi stessi.

In entrambe le funzioni, gli alberi ricevuti come parametri sono istanze della classe ricorsiva `Tree` riportata di seguito.

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
    n = input()
    node = Tree(int(n))
    nodes[n] = node
    adj[n] = list()
    if root == None:
      root = n
      tree = node

  nedges = int(input())
  for _ in range(nedges):
    src, dest = input().split()
    adj[src].append(dest)

  worklist = [root]
  while worklist:
    current = worklist.pop(0)
    node = nodes[current]
    for dest in adj[current]:
      tree.add_node(node, nodes[dest])
      worklist.append(dest)

  tasks = int(input())
  for _ in range(tasks):
    n1, n2 = input().split()
    print(least_common_ancestor(tree, int(n1), int(n2)))
```

## Testcases

### 1

IN:
```
11
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
10
1 2
1 3
1 4
2 5
2 6
3 7
4 8
4 9
4 10
6 11
5
5 6
7 4
4 9
11 11
7 9
```

OUT:
```
2
1
4
11
1
```

### 2

IN:
```
32
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
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
31
1 2
1 3
1 4
3 5
4 6
4 7
4 8
4 9
5 10
5 11
6 12
7 13
7 14
8 15
9 16
9 17
9 18
14 19
14 20
14 21
16 22
16 23
16 24
16 25
18 26
24 27
26 28
26 29
27 30
30 31
30 32
5
19 16
28 29
30 5
12 11
26 27
```

OUT:
```
4
26
1
1
9
```

## Solution

```py
def descendants(tree):
  result = set()
  for c in tree.get_children():
    result |= descendants(c)
  result.add(tree.get_data())
  return result

def least_common_ancestor(tree, n1, n2):
  for c in tree.get_children():
    lca = least_common_ancestor(c, n1, n2)
    if lca is not None:
      return lca
  d = descendants(tree)
  if n1 in d and n2 in d:
    return tree.get_data()
  return None
```
