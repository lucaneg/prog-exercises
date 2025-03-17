# Maximum Depth of Binary Tree

Write a function `max_depth(tree)` that implements a DFS algorithm to find the maximum depth of a binary tree. The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

## Template

```py
class BinaryNode:
  def __init__(self, data):
    self.data = data
    self.left = None
    self.right = None

  def __str__(self):
    return str(self.data)

  def set_left_child(self, child):
    self.left = child

  def set_right_child(self, child):
    self.right = child

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

class BinaryTree:
  def __init__(self, root):
    self.root = root

  def set_left_child(self, father, child):
    father.set_left_child(child)

  def set_right_child(self, father, child):
    father.set_right_child(child)

  def get_root(self):
    return self.root

  def __str__(self):
    return self.str_aux(self.root, '')

  def str_aux(self, node, indent):
    if node.is_leaf():
      return str(node.get_data())
    padding = indent.replace('l', '|  ').replace('r', '   ')
    if node.get_left_child() is not None:
      left_str = self.str_aux(node.get_left_child(), indent + 'l')
    else:
      left_str = '(None)'
    if node.get_right_child() is not None:
      right_str = self.str_aux(node.get_right_child(), indent + 'r')
    else:
      right_str = '(None)'
    return f'{node.get_data()}\n{padding}+- {left_str}\n{padding}+- {right_str}'

if __name__ == "__main__":
  nnodes = int(input())
  adj = dict()
  nodes = dict()
  root = None
  tree = None

  for _ in range(nnodes):
    n = input()
    node = BinaryNode(int(n))
    nodes[n] = node
    adj[n] = list()
    if root == None:
      root = n
      tree = BinaryTree(node)

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

  print(max_depth(tree))
```

## Testcases

### 1

IN:
```
9
8
3
1
6
4
7
10
14
13
7
8 l 3
8 r 10
3 r 6
6 l 4
6 r 7
10 r 14
14 l 13
```

OUT:
```
4
```

### 2

IN:
```
3
8
3
10
2
8 l 3
8 r 10
```

OUT:
```
2
```

## Solution

```py
def max_depth(tree):
  worklist = [(0, tree.get_root())]
  max = 0
  while worklist:
    depth, current = worklist.pop(0)
    if not current:
      continue
    if depth + 1 > max:
      max = depth + 1
    worklist.append((depth + 1, current.get_left_child()))
    worklist.append((depth + 1, current.get_right_child()))
  return max
```
