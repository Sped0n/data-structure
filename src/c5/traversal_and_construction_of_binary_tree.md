# 二叉树的遍历与构造

## 二叉树的遍历

二叉树遍历其实很简单，三种遍历都是一样的，只不过visit结点顺序先后不一样罢了。

### 先序遍历

访问根结点，先序遍历左子树，先序遍历右子树

```c
void PreOrderTraverse(BT &T, void (*visit)(BTNode &TNode)) {
    if (T != NULL) {
        visit(T);
        PreOrderTraverse(T -> lchild);
        PreOrderTraverse(T -> rchild);
    }
}
```

### 中序遍历

中序遍历左子树；访问根结点，中序遍历右子树

```c
void InOrderTraverse(BT &T, void (*visit)(BTNode &TNode)) {
    if (T != NULL) {
        InOrderTraverse(T -> lchild);
        visit(T);
        InOrderTraverse(T -> rchild);
    }
}
```

### 后序排列

后序遍历左子树，后序遍历右子树，访问根结点

```c
void PostOrderTraverse(BT &T, void (*visit)(BTNode &TNode)) {
    if (T != NULL) {
        PostOrderTraverse(T -> lchild);
        PostOrderTraverse(T -> rchild);
        visit(T);
    }
}
```

### 三种递归遍历的时间复杂度分析

时间复杂度都为$\mathrm{O(n)}$，在递归遍历中，递归工作栈的栈深恰好为树的深度，在最坏情况下，二叉树是有`n`个结点且深度为`n`的单支树，遍历算法的空间复杂度为$\mathrm{O(n)}$。

> 这个函数不是发生了$2^n$次递归吗，为什么复杂度是$\mathrm{O(n)}$？
>
> 简化的思考方式：就是二叉树的遍历的话，会**「每一个节点都访问一次，且仅访问一次，所以它的时间复杂度就是O(n)」**

### 利用非递归方式遍历二叉树

借助栈，可以将二叉树的递归遍历转换为非递归算法。以中序遍历为例，给出中序遍历非递归算法。先扫描（并非访问）根结点的所有左结点并将它们一一进栈，然后出栈一个结点`*p`，访问它。然后继续扫描该结点的右孩子结点，将其进栈，在扫描该孩子结点的所有左结点并一一进栈，如此继续，直到栈空。

> 步骤：
>
> 1. 扫描所有左孩子，把所有左孩子压入stack；
> 2. 扫完了以后，开始出栈；
> 3. 访问出栈元素，再扫描它的右孩子；
> 4. 在右孩子的基础上，再扫描这个右孩子的所有左结点进栈；
> 5. 回到第2步，直到没有左孩子可以进栈（栈空），此时扫描完成，退出程序。

* **前序遍历**的非递归算法：

  ```c
  void PreOrderTraverseWithoutRecursion(BT &T, void(*visit)(BTNode &TNode)) {
      if (T == NULL) {
          return ;
      }
      Stack<BT> S;
      BTNode *temp = T;
      while (temp != NULL || !IsEmpty(S)) {
          // 填栈，访问左孩子
          if (temp != NULL) {
              visit(temp);   // 先visit，再访问左，之后访问右孩子
              S.Push(temp);
              temp = temp -> lchild;
          }
          // 出栈，访问右孩子
          else {
              S.Pop(temp);
              temp = temp -> rchild;
          }
          
      }
  }
  ```

* **中序遍历**的非递归算法：

  ```c
  void InOrderTraverseWithoutRecursion(BT &T, void(*visit)(BTNode &TNode)) {
      if (T == NULL) {
          return;
      }
      Stack<BT> S;
      BTNode *temp = T;
      while (temp != NULL || !IsEmpty(S)) {
          // 填栈，访问左孩子
          if (temp != NULL) {
              S.Push(temp);
              temp = temp -> lchild;
          } 
          // 出栈，访问右孩子
          else {
              S.Pop(temp);
              visit(temp);   // 中序排列，前面访问完左，现在visit，之后访问右孩子
              temp = temp -> rchild;
          }
      }
  }
  ```

* **后序遍历**则需要另外情况说明：

  > 后序遍历的难点在于：需要判断上次访问的节点是位于左子树，还是右子树。若是位于左子树，则需跳过根节点，先进入右子树，再回头访问根节点；若是位于右子树，则直接访问根节点。直接看代码，代码中有详细的注释。
  ```c
  void PostOrderTraverseWithoutRecursion(BT &T, void(*visit)(BTNode &TNode)) {
      if (T = NULL) {
          return;
      }
      Stack<BT> S;
      BTNode *temp = T;
      BTNode *lastVisit = NULL;
      // 第一次压点
      while (temp) {
          S.Push(temp);
          temp = temp -> lchild;
      }
      // 回溯逻辑
      while (!IsEmpty(S)) {
          S.Pop(temp);
          // 上一个访问结点的是现在这个结点右结点，或者没有右结点，那就准备访问
          if (cur -> rchild == NULL || cur -> rchild = lastVisit) {
              visit(temp);
              lastVisit = temp;
          }
          // 反之，上一个访问的结点是现在这个结点的左结点，那现在不准访问，先继续遍历，直到满足上面的条件
          else {
              S.Push(temp);
              temp = temp -> rchild;
              while (temp != NULL) {
                  S.Push(temp);
                  temp = temp -> lchild;
              }
          }
      }
  }
  ```

### 利用队列的方式进行层次遍历

层次遍历需要借助队列。先将二叉树根结点入队，然后出队，访问该结点，若它有左子树，则将左子树根节点入队；若它有右子树，则将右子树根结点入队。然后出队，对出队结点访问，如此反复，直到队列为空。

![img](https://img.sped0nwen.com/image/2023/06/07/xm2vhz-0.webp)

```c
void levelOrder(BT &T, void(*visit)(BTNode &TNode)) {
    if (T = NULL) {
        return;
    }
    Queue<BT> Q;
    Q.Push(T);
    while (!IsEmpty(Q)) {
        int QSize = GetSize(Q);
        BTNode *temp;
        for(int i = 0; i < QSize; i++) {
            temp = pop(Q);
            // visit(temp); 前序
            if (temp -> lchild != NULL) {
                Q.Push(temp -> lchild);
            }
            // visit(temp); 中序
            if (temp -> rchild != NULL) {
                Q.Push(temp -> rchild);
            }
            // visit(temp); 后序
        }
    }
}
```

## 由遍历序列构造二叉树

由二叉树的先序和中序序列可以唯一确定一棵二叉树，同样后序和中序序列也可以唯一确定一棵二叉树，但<u>先序和后序不能确定一棵二叉树</u>。

### 先序和中序序列构造一棵二叉树

思路：我们知道先序序列的第一个值其实就是根节点的值，这样我们就可以在中序序列中找到对应的根节点，那么根节点左边的数就全为左子树的结点，根节点右边的数就全为右子树的结点。根据这个想法，再递归的进行构造，见代码。

以下是实现的思路：

1. 先序序列的第一个元素是当前树的根节点。
2. 在中序序列中查找根节点，根节点左边的元素是当前树的左子树，右边的元素是当前树的右子树。(如果不是同时有左右孩子，则作废)
3. 递归构造左子树和右子树。

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct BTNode {
    Elemtype data;
    struct BTNode *lchild, *rchild;
}BTNode, *BT;

int len(int &list) {
    if (list == NULL) {
        return -1;
    }
    return sizeof(list) / sizeof(list[0]);
}

int findIndex(int &list, int val){
    int count = len(list)
    for (int i = 0; i < count; i++) {
        if (list[i] == val) {
            return i;
        }
    }
    return -1;
}

BTNode *BuildTree(int &preorder, int &inorder, int pL, int pR, int iL, int iR) {
    int rootIndex = findIndex(inorder, preorder[pL]);
    BTNode *root = (BTNode*)malloc(sizeof(BTNode));
    root -> data = preorder[pL];
    int leftCount = rootIndex - iL;
    // 递归前序遍历第二个到左半，中序遍历左半
    root -> left = BuildTree(preorder, inorder, pL + 1, pL + leftCount, iL, rootIndex - 1);
    // 递归前序遍历右半，中序遍历中间到右半
    root -> right = BuildTree(preorder, inorder, pL + leftCount + 1, pR, rootIndex + 1, iR);
    return root;
}

void main {
    BuildTree(preorder, inorder, 0, len(preorder) - 1, 0, len(inorder) - 1);
}
```

### 后序和中序构建

我们知道后序序列的最后一个值其实就是根节点的值，这样我们就可以在中序序列中找到对应的根节点，那么根节点左边的数就全为左子树的结点，根节点右边的数就全为右子树的结点。根据这个想法，再递归的进行构造，见代码。

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct BTNode {
    Elemtype data;
    struct BTNode *lchild, *rchild;
}BTNode, *BT;

int len(int &list) {
    if (list == NULL) {
        return -1;
    }
    return sizeof(list) / sizeof(list[0]);
}

int findIndex(int &list, int val){
    int count = len(list)
    for (int i = 0; i < count; i++) {
        if (list[i] == val) {
            return i;
        }
    }
    return -1;
}

BTNode *BuildTree(int &postorder, int &inorder, int pL, int pR, int iL, int iR) {
    int rootIndex = findIndex(inorder, postorder[pR]);
    BTNode *root = (BTNode*)malloc(sizeof(BTNode));
    root -> data = postorder[pR];
    int rightCount = iR - rootIndex;
    // 递归前序遍历第二个到左半，中序遍历左半
    root -> left = BuildTree(postorder, inorder, pL, pR - rightCount - 1, iL, rootIndex - 1);
    // 递归前序遍历右半，中序遍历中间到右半
    root -> right = BuildTree(postorder, inorder, pR - rightCount, pR - 1, rootIndex + 1, iR);
    return root;
}

void main {
    BuildTree(preorder, inorder, 0, len(preorder) - 1, 0, len(inorder) - 1);
}
```

