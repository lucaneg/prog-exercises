# Writing trees

Using the provided Tree class, write a function `build_tree()` that creates a Tree object representing the tree of Humans and returns it:

![tree](https://upload.wikimedia.org/wikipedia/commons/f/fc/Hominoid_taxonomy_7.svg)

## Template

```py
class Node:
  def __init__(self, data):
    self.data = data
    self.children = []

  def __str__(self):
    return str(self.data)

  def add_child(self, child):
    self.children.append(child)

  def get_children(self):
    return self.children

  def is_leaf(self):
    # 'len(self.children) != 0' is slower
    return not self.children

  def get_data(self):
    return self.data

  def set_data(self, data):
    self.data = data

class Tree:
  def __init__(self, root):
    # contrary to the linked list, the root is mandatory
    self.root = root

  def add_node(self, father, child):
    father.add_child(child)

  def get_root(self):
    return self.root

  def __str__(self):
    return self.str_aux(self.root, '')

  def str_aux(self, node, indent):
    if node.is_leaf():
      return str(node.get_data())
    padding = indent.replace('c', '|  ').replace('l', '   ')
    ch_str = ''
    for c in node.get_children():
      ch_str += padding + '+- ' + self.str_aux(c, indent + ('c' if node.get_children().index(c) != len(node.get_children()) - 1 else 'l')) + '\n'
    return (f'{node.get_data()}\n{ch_str}').strip()

if __name__ == '__main__':
    print(build_tree())
```

## Testcases

### 1

IN:
```
0
```

OUT:
```
hominoidea
+- hominidae
|  +- hominiae
|  |  +- hominini
|  |  |  +- homo
|  |  |  +- pan
|  |  +- gorillini
|  |     +- gorilla
|  +- ponginae
|     +- pongo
+- hylobatidae
```

## Solution

```py
def build_tree():
    hominoidea = Node('hominoidea')
    hominidae = Node('hominidae')
    hominiae = Node('hominiae')
    hominini = Node('hominini')
    homo = Node('homo')
    pan = Node('pan')
    gorillini = Node('gorillini')
    gorilla = Node('gorilla')
    ponginae = Node('ponginae')
    pongo = Node('pongo')
    hylobatidae = Node('hylobatidae')

    tree = Tree(hominoidea)
    tree.add_node(hominoidea, hominidae)
    tree.add_node(hominoidea, hylobatidae)
    tree.add_node(hominidae, hominiae)
    tree.add_node(hominidae, ponginae)
    tree.add_node(ponginae, pongo)
    tree.add_node(hominiae, hominini)
    tree.add_node(hominiae, gorillini)
    tree.add_node(hominini, homo)
    tree.add_node(hominini, pan)
    tree.add_node(gorillini, gorilla)
    
    return tree
```
