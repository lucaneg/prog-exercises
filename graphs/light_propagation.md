# Propagazione della luce

Una casa è modellata tramite un grafo in cui ogni nodo rappresenta una stanza, e ogni arco rappresenta una porta che collega le due stanze. Alcune delle stanze hanno una luce accesa al loro interno, mentre altre sono completamente buie. Si suppone che, ad ogni istante di tempo, la luce si propaghi da una stanza illuminata a tutte le stanze adiacenti (cioè collegate tramite un arco) che sono attualmente buie. Le stanze già accese in partenza sono illuminate all'istante 0, le stanze adicenti sono illuminate all'istante 1, ...

Scrivere una funzione `light_spread(house, lit_rooms)` che, dato un grafo (definito ricorsivamente tramite la classe `GraphWithAdjacencyList`) rappresentate una casa, e data una lista di nodi del grafo che rappresentano le stanze in cui è presente una luce accesa, ritorni il numero di istanti di tempo necessari per illuminare l'intera casa.

Ad esempio, dato il grafo:
```
A --- B --- C --- D
|     |           |
|     |           |
E --- F --------- G
```
e supponendo che le stanze A e F abbiano una luce accesa al loro interno, si ha:
- A e F sono illuminate all'istante 0
- E, B, F sono illuminate all'istante 1
- C e D sono illuminate all'istante 2

`light_spread` deve quindi ritornare 2.

Usare una BFS. Se necessario, è possibile aggiungere parametri aggiuntivi alla funzione, purché abbiano dei valori di default.

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
  
  rooms = input().split()

  print(light_spread(graph, rooms))
```

## Testcases

### 1

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
B C
C D
A E
B F
D G
E F
F G
A F
```

OUT:
```
2
```

### 2

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
A
```

OUT:
```
1
```

### 3

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
A D
```

OUT:
```
2
```

### 4

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
A C
```

OUT:
```
3
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
B E
```

OUT:
```
1
```

### 6

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
A E F
```

OUT:
```
2
```

## Solution

```py

```
