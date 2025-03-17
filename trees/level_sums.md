# Level sums

In a binary tree, a **level sum** is the sum of all the data inside nodes on the same level, that is, at the same depth from the root. For instance, given the following tree:

![image.png](https://raw.githubusercontent.com/lucaneg/inf1mod2/master/tree_example_ex4.png)

The level sum of level 0 is 7, as the level only contains the root. Instead, the level sum of level 2 is `9 + 6 + 2 + 3 = 20`.

Write a function `level_sum(tree)` that calculates the sum of data at each level of a binary tree, and returns all the sums as a list where each element correspons to the sum of values at a particular level (e.g., the value in position 2 is the level sum of level 2).

**Hint:** since we want to proceed by level, there is one type of visit tht is a perfect match for this exercise.

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

  def __str__(self):
    return self.str_aux('')

  def str_aux(self, indent):
    if self.is_leaf():
      return str(self.get_data())
    padding = indent.replace('l', '|  ').replace('r', '   ')
    if self.get_left_child() is not None:
      left_str = self.get_left_child().str_aux(indent + 'l')
    else:
      left_str = '(None)'
    if self.get_right_child() is not None:
      right_str = self.get_right_child().str_aux(indent + 'r')
    else:
      right_str = '(None)'
    return f'{self.get_data()}\n{padding}+- {left_str}\n{padding}+- {right_str}'

if __name__ == '__main__':
  nnodes = int(input())
  adj = dict()
  nodes = dict()
  root = None
  tree = None

  for _ in range(nnodes):
    n = input()
    node = BinaryTree(int(n))
    nodes[n] = node
    adj[n] = list()
    if root == None:
      root = n
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

  print(level_sum(tree))
```

## Testcases

### 1

IN:
```
7
7
4
15
9
6
2
3
6
7 l 4
7 r 15
4 l 9
4 r 6
15 l 2
15 r 3
```

OUT:
```
[7, 19, 20]
```

### 2

IN:
```
4
9
-2
-8
3
3
9 l -2
9 r -8
-8 l 3
```

OUT:
```
[9, -10, 3]
```

### 3

IN:
```
1
5
0
```

OUT:
```
[5]
```

### 4

IN:
```
10
1
0
6
4
-22
12
74
-3
29
81
9
1 l 0
1 r 6
0 l 4
0 r -22
6 r 12
-22 l 74
12 l -3
74 l 29
74 r 81
```

OUT:
```
[1, 6, -6, 71, 110]
```

## Solution

```py
def level_sum(tree):
  result = []
  worklist = [(tree, 0)]
  current_level_sum = 0
  while len(worklist) > 0:
    current, level = worklist.pop(0)
    current_level_sum += current.get_data()
    if len(worklist) == 0 or worklist[0][1] != level:
      result.append(current_level_sum)
      current_level_sum = 0
    if current.get_left_child() is not None:
      worklist.append((current.get_left_child(), level + 1))
    if current.get_right_child() is not None:
      worklist.append((current.get_right_child(), level + 1))
  return result
```
