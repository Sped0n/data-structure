# 树和森林

## 树的存储结构

树的存储方式有很多种，既可采用顺序结构，又可采用链式存储结构，但无论采用何种存储方式，都要求能唯一的反映树中各结点之间的逻辑关系。下面是常见的存储结构：

### 双亲表示法

这种存储方式采用<u>一组连续空间</u>来存储每个结点，同时在每个结点中增设一个<u>伪指针，指示其双亲结点在数组中的位置</u>。

![img](https://img.sped0nwen.com/image/2023/06/13/we1sy2-0.webp)

```c
#define MAX_TREE_SIZE 100
typedef struct {
    Elemtype data;
    int parent;    // 双亲在list中的位置
}PTNode；
    
typedef struct {
    PTNode nodes[MAX_TREE_SIZE];
    int root, num; // 根结点的位置和结点的个数
}
```

**特点：找双亲容易，找孩子难**

### 孩子表示法

孩子表示法是将<u>每个结点的孩子结点都用单链表链接起来形成一个线性结构</u>，此时n个结点就有n个孩子链表（叶子结点的孩子链表尾空表）。

![img](https://img.sped0nwen.com/image/2023/06/13/x5th2l-0.webp)

### 孩子兄弟表示法

孩子兄弟表示法又称二叉树表示法，即以二叉链表作为树的存储结构。孩子兄弟表示法使每个结点包括三部分内容：结点值，指向结点第一个孩子结点的指针，指向结点下一个兄弟结点的指针（沿此可以找到结点的所有兄弟结点）。

![img](https://img.sped0nwen.com/image/2023/06/13/x8710x-0.webp)

孩子兄弟表示法的存储结构：

```c
typedef struct CSNode {
    Elemtype data;
    CSNode *firstchild, *nextsibling;
}CSNode, *CSTree
```

**特点：操作容易，但破坏了树的层次，从当前结点查找双亲结点比较麻烦**

## 树，森林与二叉树的转换

### 树转化为二叉树的规则

每个结点左指针指向它的第一个孩子结点，右指针指向它在树中的相邻兄弟结点，可即为“左孩子右兄弟”。由于根结点没有兄弟，所以由树转换而得的二叉树没有右子树。

![img](https://img.sped0nwen.com/image/2023/06/13/x9asbq-0.webp)

### 森林转换为二叉树的原则

先将每一棵树转换为二叉树，再将第一棵树的根作为转换后的二叉树的根，将第一棵树的左子树作为转换后的左子树，将第二棵树作为转换后的右子树，将第三棵树作为转换后的二叉树的右子树的右子树，以此类推。

![img](https://img.sped0nwen.com/image/2023/06/13/x9we8j-0.webp)

### 二叉树转换为森林的原则

若二叉树非空，则二叉树的根及其左子树为第一棵树的二叉树形式，二叉树的根的右子树可视为一个由除第一棵树外的森林转化后的二叉树，用同样的方法，知道最后产生一棵没有右子树的二叉树为止。

![img](https://img.sped0nwen.com/image/2023/06/13/xd16e2-0.webp)

将原先的左子树孩子节点转换为左子树孩子结点不变，原先的兄弟结点转换为左孩子结点的右子树。

## 树和森林的遍历

### 树的遍历

1. **先根遍历：**

   若树非空，先访问根结点，再按从左到右的顺序遍历根结点的每棵子树。和相对应的二叉树的先序遍历顺序相同

2. **后根遍历：**

   若树非空，先按从左到右的顺序遍历根结点的每棵子树，之后在访问根结点。和相对应的二叉树的中序遍历顺序相同

3. **层次遍历**：

   与二叉树的层次遍历思想基本相同

![image-20230613202359821](https://img.sped0nwen.com/image/2023/06/13/xh4309-0.webp)

### 森林的遍历

1. **先序遍历森林：**

   若森林非空，访问森林中第一棵树的根结点；先序遍历第一课树中根结点的子树森林；先序遍历除去第一棵树之后剩余的树构成的森林。

   > 即:**依次从左至右**对森林中的每一棵**树**进行**先根遍历**。

2. **后序遍历森林：**

   若森林非空，后序遍历森林中第一棵树的子树森林；访问森林中第一棵树的根结点；后序遍历森林中除去第一棵树之后剩余的树构成的森林。

   > 即:**依次从左至右**对森林中的每一棵**树**进行**后根遍历**。