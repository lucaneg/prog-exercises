# Maximum path sum

In a binary tree, a **path sum** is the sum of all the data inside nodes of a root-to-leaf path. For instance, given the following tree:

![image.png](https://raw.githubusercontent.com/lucaneg/inf1mod2/master/tree_example_ex4.png)

The path sum from the root 7 to the leaf 6 is `7 + 4 + 6 = 17`, while the path sum from root 7 to leaf 2 is `7 + 15 + 2 = 24`.

Write a function `get_max_path_sum` that, taken a binary tree as its only parameter, returns the maximum path sum among all its root-to-leaf paths. You can assume that the nodes will only contain numeric values as data.

**Hint:** since the paths are root-to-leaf, there is one type of visit that is a perfect match for this exercise.

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

  print(get_max_path_sum(tree))
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
25
```

### 2

IN:
```
1
5
0
```

OUT:
```
5
```

### 3

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
134
```

### 4

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
7
```

## Solution

```py
def get_max_path_sum(tree):
  if tree is None:
    return 0
  lsum = get_max_path_sum(tree.get_left_child())
  rsum = get_max_path_sum(tree.get_right_child())
  return tree.get_data() + max(lsum, rsum)
```
