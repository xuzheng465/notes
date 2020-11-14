# 中序遍历 Morris



空间复杂度为O(1)

```python
# 递归版本
def inorder(root):
  inorder(root.left)
  print(root)
  inorder(root.right)
```



## 步骤

1. 如果x无左孩子, 先将x的值加入答案数组, 再访问x的右孩子, 即x=x.right
2. 如果x有左孩子, 则找到左子树上最右的节点, 即左子树中序遍历的最后一个节点, 即x在中序遍历中的前驱节点, 标记为predecessor. 根据predecessor的右孩子是否为空,进行如下操作
   1. 若predecessor的右孩子为空, 则将其右孩子指向x, 访问x的左孩子, 即x=x.left
   2. 若右孩子不为空, 则此时其右孩子指向x,  说明我们已经遍历完x的左子树, 将predecessor的右孩子指向null, 将x的值加入答案数组, 然后x向右移动 即x=x.right
3. 重复上述操作直至访问完整.



