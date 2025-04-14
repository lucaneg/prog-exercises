# Grafo bipartito

Un grafo si dice bipartito se è possibile colorare i suoi vertici con due colori in modo che ogni vertice abbia un colore diverso da tutti i suoi vicini. Ad esempio, il seguente grafo è bipartito:

![image.png](https://raw.githubusercontent.com/lucaneg/inf1mod2/master/bipartite.png)

In quanto è possibile colorare i vertici in maniera alternata: ogni nodo rosso ha solo vicini blu, e viceversa.

![image.png](https://raw.githubusercontent.com/lucaneg/inf1mod2/master/bipartite-colored.png)

Al contrario, il seguente grafo non è bipartito: se il nodo 1 è rosso e il nodo 2 è blu, il nodo 3 condividerà sempre il colore con uno dei suoi vicini.

![image.png](https://raw.githubusercontent.com/lucaneg/inf1mod2/master/non-bipartite.png)

Scrivere la funzione `is_bipartite(graph, start)` che, dato un grafo e un suo nodo di partenza, ritorna `True` se il grafo è bipartito, altrimenti `False`. `is_bipartite` deve essere implementata come una **BFS** che:
- rappresenta il colore blu come il Booleano `True` e il colore rosso come Booleano `False`
- tiene traccia dei colori dei nodi in un dizionario (che mappa quindi nodi a `True`/`False`): colorare un nodo significa quindi inserirlo nel dizionario
- inizia la visita dal nodo `start`, che va sempre colorato di blu *prima di iniziare la visita*
- per ogni nodo processato, esamina ogni successore individualmente:
  - se il successore non è ancora stato colorato, lo colora con il colore opposto 
  - se il successore è già stato colorato ed ha colore opposto, lo salta
  - se il successore è già stato colorato ed ha lo stesso colore del nodo corrente, la funzione termina e ritorna `False`
- al termine della visita, ritorna `True`


## Template

```py
class GraphWithAdjacencyList:
  def __init__(self, directed=True, weighted=False):
    self.adjlist = dict()
    self.directed = directed
    self.weighted = weighted

  def add_vertex(self, vertex):
    self.adjlist[vertex] = dict()

  def remove_vertex(self, vertex):
    del self.adjlist[vertex]
    for neighbors in self.adjlist.values():
      del neighbors[vertex]

  def add_edge(self, src, dest, weight=1):
    self.adjlist[src][dest] = weight
    if not self.directed:
      self.adjlist[dest][src] = weight

  def remove_edge(self, src, dest):
    del self.adjlist[src][dest]
    if not self.directed:
      del self.adjlist[dest][src]

  def get_successors(self, v):
    return list(self.adjlist[v].keys())

  def get_predecessors(self, v):
    predecessors = []
    for vertex in self.adjlist:
      if v in self.get_successors(vertex):
        predecessors.append(vertex)
    return predecessors

  def get_edge_weight(self, src, dest):
    if not self.weighted:
      return None
    else:
      return self.adjlist[src][dest]

  def set_edge_weight(self, src, dest, weight):
    if self.weighted:
      self.adjlist[src][dest] = weight

  def get_vertexes(self):
    return list(self.adjlist.keys())

  def get_edges(self):
    edges = []
    for vertex, successors in self.adjlist.items():
      for succ in successors:
        if not self.weighted:
          edges.append((vertex, succ))
        else:
          edges.append((vertex, succ, successors[succ]))
    return edges

  def __str__(self):
    edges = []
    for e in self.get_edges():
      if not self.weighted:
        edges.append(str(e[0]) + ' -> ' + str(e[1]))
      else:
        edges.append(str(e[0]) + ' -> ' + str(e[1]) + ' w=' + str(e[2]))

    return 'Vertexes: ' + ', '.join([str(v) for v in self.get_vertexes()]) + \
      '\nEdges: ' + ', '.join(edges)

if __name__ == '__main__':
  nnodes = int(input())
  graph = GraphWithAdjacencyList(directed=False, weighted=False)

  for _ in range(nnodes):
    graph.add_vertex(input())

  nedges = int(input())
  for _ in range(nedges):
    src, dest = input().split()
    graph.add_edge(src, dest)

  print(is_bipartite(graph, 'A'))
```

## Testcases

### 1

IN:
```
3
A
B
C
3
A B
B C
C A
```

OUT:
```
False
```

### 2

IN:
```
10
A
B
C
D
E
F
G
H
I
J
11
A B
A G
B D
C E
C J
D J
E F
F G
F I
H I
H J
```

OUT:
```
True
```

### 3

IN:
```
10
A
B
C
D
E
F
G
H
I
J
12
A B
A C
A G
B D
C E
C J
D J
E F
F G
F I
H I
H J
```

OUT:
```
False
```

### 4

IN:
```
5
A
B
C
D
E
5
A B
A C
B D
B E
C D
```

OUT:
```
True
```

### 5

IN:
```
6
A
B
C
D
E
F
7
A B
A F
B C
C D
C F
D E
E F
```

OUT:
```
True
```

### 6

IN:
```
7
A
B
C
D
E
F
G
8
A B
A C
A D
B E
C D
C E
E F
F G
```

OUT:
```
False
```

## Solution

```py
def is_bipartite(graph, start):
  colors = {start: True}
  worklist = [start]
  while worklist:
    current = worklist.pop(0)
    color = colors[current]
    for s in graph.get_successors(current):
      if s not in colors:
        colors[s] = not color
        worklist.append(s)
      elif color != colors[s]:
        continue
      else:
        return False
  return True
```
