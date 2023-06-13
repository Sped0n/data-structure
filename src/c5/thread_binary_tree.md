# 线索二叉树

## 基本概念

遍历二叉树以一定规则将二叉树中的结点排列成一个线性序列，得到二叉树结点的各种遍历序列，其实质是对一个非线性结构进行线性化操作，使这个访问序列中的每个结点（第一个和最后一个除外）都有一个直接前驱和直接后继。

引入线索二叉树是为了加快查找结点前驱和后继的速度。在有n的结点的二叉树中，有n+1个空指针。

**在二叉树线索化时，通常规定：若无左子树，令lchild指向其前驱结点；若无右子树，令rchild指向后继结点。还需要增加两个标志域表明当前指针域所指对象是指向左（右）子结点还是指向直接前驱（后继）。**

![img](https://img.sped0nwen.com/image/2023/06/13/w082r6-0.webp)

线索二叉树的存储结构描述如下：

```c
typedef struct ThreadNode{
    Elemtype data;
    struct ThreadNode *lchild, *rchild;
    bool ltag, rtag;
}ThreadNode, *ThreadTree
```

以这种结点构成的二叉链表作为二叉树的存储结构，称为**线索链表**，其中指向结点前驱和后继的指针称为**线索**。加上线索的二叉树称为**线索二叉树**。对二叉树以某种次序遍历使其变为线索二叉树的过程称为**线索化**。

## 线索二叉树的构造

对二叉树的线索化，实质上就是遍历一次二叉树，只是在遍历的过程中，检查当前节点的左右指针域是否为空，若为空，将它们改为指向前驱结点或指向后继结点的线索。

**线索化的实质就是将二叉链表中的空指针改为指向前驱或后继的线索。由于前驱和后继信息只有在遍历该二叉树时才能得到，所以，线索化的过程就是在遍历的过程中修改空指针的过程。**

![img](https://img.sped0nwen.com/image/2023/06/13/w0bw6u-0.webp)

以中序遍历对二叉树线索化的递归算法为例：

**前驱结点：**

1. 若左指针为线索，则其指向结点为前驱结点
2. 若左指针为左孩子，则其左子树的最右侧结点为前驱结点

**后继结点：**

1. 若右指针为线索，则其指向结点为后继结点
2. 若右指针为右孩子，则其右子树的最左侧结点为后继结点

```c
void InThread(ThreadTree &p, ThreadTree &pre) { //线索二叉树的根结点，指向前驱结点的指针
    if (p != NULL) {
        InThread(p -> lchild, pre);       //递归，线索化左子树
        if (p -> lchild == NULL) {        //左子树为空，建立前驱线索
            p -> lchild == pre;
            p -> ltag = true;
        }
        if (pre != NULL && pre -> rchild == NULL) {
            pre -> rchild = p;          //建立前驱结点的后继线索
            pre -> rtag = 1;
        }
        pre = p;                       //标记当前结点成为刚刚访问过得结点
        InThread(p -> rchild, pre);    //递归，线索化右子树
    }    
}

void CreateInThread(ThreadTree &T){
    ThreadTree pre = NULL;
    if (T != NULL) {                 //非空二叉树线索化
        InThread(T, pre);            //线索化二叉树
        pre -> rchild = NULL;        //处理遍历的最后一个结点
        pre -> rtag = 1;
    }    
}
```

有时为了方便，仿照线性表的存储结构，在二叉树的线索链表上也添加一个头结点，并令其lchild指针指向二叉树的二叉树的根结点，令 rchild 指针指向中序遍历时访问的最后一个结点；令中序遍历的第一个节点的lchild指针和最后一个结点的rchild指针均指向头结点。这就好比为二叉树建立了一个双向线索链表，既可以从第一个结点顺后继进行遍历，也可以从最后一个结点顺前驱进行遍历。

## 线索二叉树的遍历

1. 求中序线索二叉树中中序序列的第一个结点：

   ```c
   ThreadNode* FirstNode(ThreadNode &p) {
       while (p -> ltag == 0) {
           p = p -> lchild;
       }
       return p;   
   }
   ```

2. 求中序线索二叉树中结点p在中序序列下的后继结点：

   ```c
   ThreadNode* NextNode(ThreadNode &p) {
       if (p -> rtag == 0) {
           return FirstNode(p -> rchild);
       } else {
           return p -> rchild;
       }
   }
   ```

3. 中序遍历

   ```c
   void InOrder(ThreadNode &T, void(*visit)(ThreadNode &T)){
       for (ThreadNode *p = FirstNode(T); p != NULL; p = NextNode(p)){
           visit(p);
       }
   }
   ```

4. 中序线索二叉树的最后一个结点：

   ```c
   ThreadNode* LastNode(ThreadNode &p) {
       while (p -> rtag == 0) {
           p = p -> rchild;
       }
       return p;
   }
   ```

5. 求中序线索二叉树中结点p在中序序列下的前驱结点：

   ```c
   ThreadNode* PreNode(ThreadNode &p) {
       if (p -> ltag == 0) {
           return FirstNode(p -> lchild);
       } else {
           return p -> lchild;
       }    
   }
   ```

   
