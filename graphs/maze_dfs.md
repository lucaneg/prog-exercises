# Risoluzione di un labirinto, versione a grafo

Un labirinto come quello in figura può essere rappresentato attraverso un grafo, dove i nodi rappresentano le celle del labirinto e gli archi rappresentano i possibili movimenti da una cella all'altra.

![maze](https://www.geeksforgeeks.org/wp-content/uploads/ratinmaze_filled11.png)

Scrivere una funzione `solve_maze(maze, start, end)` che, dato un grafo `maze` che rappresenta un labirinto, utilizza una **DFS** per trovare il percorso *più breve* che porta dal nodo `start` al nodo `end`. Tale funzione deve ritornare una lista contenente i nodi che compongono il percorso.

**NOTA**: rispetto alla DFS classica vista a lezione, qui c'è un accorgimento in più da prendere. Un nodo del grafo non può comparire due volte sullo stesso cammino, ma può comparire in più cammini diversi. In alcune situazioni andranno presi in considerazione nodi già visitati! Su avogador c'è un solo testcase che ha bisogno di questo: preoccupatevene quando ci arriverete!

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

  src = input()
  dest = input()

  print(solve_maze(graph, src, dest))
```

## Testcases

### 1

IN:
```
35
00
01
02
03
04
05
06
10
11
12
13
14
15
16
20
21
22
23
24
25
26
30
31
32
33
34
35
36
40
41
42
43
44
45
46
16
02 12
10 11
10 20
11 12
12 13
13 14
14 24
20 30
24 34
30 31
31 32
32 42
34 35
35 45
42 43
45 46
02
46
```

OUT:
```
['02', '12', '13', '14', '24', '34', '35', '45', '46']
```

### 2

IN:
```
16
00
01
02
03
10
11
12
13
20
21
22
23
30
31
32
33
7
00 10
10 11
11 21
21 31
31 30
31 32
32 33
00
33
```

OUT:
```
['00', '10', '11', '21', '31', '32', '33']
```

### 3

IN:
```
30
00
01
02
03
04
10
11
12
13
14
20
21
22
23
24
30
31
32
33
34
40
41
42
43
44
50
51
52
53
54
12
01 11
10 11
11 12
12 13
13 23
23 33
31 32
31 41
32 33
33 34
41 51
53 54
01
53
```

OUT:
```
None
```

### 4

IN:
```
54
00
01
02
03
04
05
06
07
08
10
11
12
13
14
15
16
17
18
20
21
22
23
24
25
26
27
28
30
31
32
33
34
35
36
37
38
40
41
42
43
44
45
46
47
48
50
51
52
53
54
55
56
57
58
32
03 13
05 15
08 18
11 21
11 12
12 13
13 23
15 16
15 25
16 17
17 18
17 27
20 21
20 30
23 33
23 24
24 25
25 35
27 37
30 40
33 43
35 45
37 38
40 50
40 41
41 42
42 43
45 55
54 55
55 56
56 57
57 58
03
38
```

OUT:
```
['03', '13', '23', '24', '25', '15', '16', '17', '27', '37', '38']
```

## Solution

```py
def solve_maze(maze, start, end):
  def dfs(graph, current, visited):
    if current in visited:
      return
    if current == end:
      return [current]
    visited.add(current)
    best = None
    for succ in graph.get_successors(current):
      res = dfs(graph, succ, visited)
      if res is not None:
        if best is None or len(res) < len(best):
          best = res
    visited.remove(current)
    if best is not None:
      return [current] + best
    return None

  return dfs(maze, start, set())
```
