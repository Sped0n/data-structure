# 图的存储结构及基本操作

<!-- toc -->

图的存储必须要完整、准确的反映顶点集和边集的信息。主要的存储方式有两种：邻接矩阵 和 邻接表。前者属于图的顺序存储结构，后者属于图的链式存储结构。

## 存储结构

### 邻接矩阵法（Adjacent Array）

#### 定义

所谓邻接矩阵存储，是指**用一个一维数组存储图中顶点的信息**，**用一个二维数组存储图中边的信息（即各顶点之间的邻接关系）**，存储顶点之间邻接关系的二维数组称为**邻接矩阵**。

结点数为 $n$ 的 图 $G = (V,E)$的邻接矩阵A 是$n\times n$的。将G的顶点编号为$V1,V2,\cdots,Vn$。
$$
A[i][j]=\left\{\begin{array}{l}
W_{i j}, \quad \text { 若 }\left(V_{i}, V_{j}\right) \text { 或 }<V_{i}, V_{j}>\text { 是 } E(G) \text { 中的边 } \\
0 \text { 或 } \infty, \quad \text { 若 }\left(V_{i}, V_{j}\right) \text { 或 }<V_{i}, V_{j}>\text { 不是 } E(G) \text { 中的边 }
\end{array}\right.
$$
对于带权树而言，若顶点$V_i$ 和 $V_j$ 之间有边相连，则邻接矩阵中对应的项存放着边相对应的权值，若顶点之间不相连，则用$\infin$来代表两个顶点之间不存在边：
$$
A[i][j]=\left\{\begin{array}{l}
W_{i j}, \quad \text { 若 }\left(V_{i}, V_{j}\right) \text { 或 }<V_{i}, V_{j}>\text { 是 } E(G) \text { 中的边 } \\
0 \text { 或 } \infty, \quad \text { 若 }\left(V_{i}, V_{j}\right) \text { 或 }<V_{i}, V_{j}>\text { 不是 } E(G) \text { 中的边 }
\end{array}\right.
$$
![img](https://img.sped0nwen.com/image/2023/06/17/mdkxr7-0.webp)

#### 数据结构

```c
#define MAXVERTICENUM 100
typedef char VerticeType;
typedef int EdgeType;
typedef struct {
    ElemType Ver[MAXVERTICENUM];
    EdgeType Edge[MAXVERTICENUM][MAXVERTICENUM];
    int VerNum, ArcNum
}AAGraph;
```

> 邻接矩阵表示法的空间复杂度为$O(n^2)$，其中$n$为图的顶点数。

#### 特点

1. **无向图的邻接矩一定是对称矩阵**（并且唯一）。因此，在实际存储邻接矩阵时**只需存储上（或下）三角矩阵的元素**。

2. 对于无向图，邻接矩阵的**第i行（或第i列）非零元素（或非∞元素）的个数正好是第 i 个顶点的度**。

3. 对于有向图，邻接矩阵的第i行（或第i列）非零元素（或非∞元素）的个数正好是第 i 个顶点的出度（或入度）。

   > i行对应i出度，i列对应i入度

4. 用邻接矩阵法存储图，很容易确定图中任意两顶点之间是否有边相连。但是，要确定图中有多少边，则必须按照行，按列，对每个元素进行检测，所花费的时间代价很大。这是用邻接矩阵存储图的局限性。

   > 确定边数困难

5. 稠密图适合用邻接矩阵表示
6. 设图G中的邻接矩阵为$A$，$A_n$ 的元素$A_{ij}$ 等于由顶点 $i$ 到顶点 $j$ 的长度为 $n$ 的路径的数目。

### 邻接列表法（Adjacent List）

当一个图为稀疏图时，使用**邻接矩阵表示法显然要浪费大量的存储空间**，而图的邻接表法结合了顺序存储和链式存储方法，大大减少了这种不必要的浪费。

#### 定义

对图中每个顶点建立一个单链表，第i个单链表中的结点表示依附于顶点的边，这个单链表就称为顶点的的边表。

边表的头指针和顶点的数据信息采用顺序存储（称为顶点表），所以在邻接表中存在两种结点：

* 顶点表结点
* 边表结点

![img](https://img.sped0nwen.com/image/2023/06/17/nb8gud-0.webp)

点表结点由 顶点域（data）和指向第一条邻接边的指针（firstarc）构成，边表（邻接表）结点由 邻接点域（adjvex）和 指向下一条邻接边的指针域（nextarc）构成。

无向图和有向图的邻接表的实例如下：

![img](https://img.sped0nwen.com/image/2023/06/17/nc9kgp-0.webp)

#### 数据结构

```c
#define MAXVERTICENUM 100
typedef struct EdgeNode {
    int AdjVerticeIndex;     // 该边表结点指向项的索引
    struct EdgeNode *next;
}EdgeNode;

typedef struct VerNode {
    Elemtype data;
    EdgeNode *first;
}VerNode, *AL[MAXVERTICENUM];
    
typedef struct {
    AL vertices;
    int VerNum, ArcNum;
}ALGraph;
```

#### 特点

1. 若G为无向图，则所需的存储空间为$O(|v|+2|E|)$；若G为有向图，则所需的存储空间为$O(|v|+|E|)$。

2. 对于稀疏图，采用邻接表表示将极大地节省存储空间。

3. 在邻接表中，给定一顶点，能很容易地找出他的所有邻边，因为只需要读取它的邻接表。在邻接矩阵中，相同操作则需要扫描一行，花费的时间为$O(n)$。但是，若要确定给定的两个顶点间是否存在边，则需在邻接矩阵中可以立刻查到，而在邻接表中则要在相应结点对应的边表中查找另一结点，效率低下。

   > 找所有邻边快，但不适合确定是否存在邻边。

4. 在有向图的邻接表表示中，求一个给定结点的出度只需计算其邻接表中的结点个数；但求其顶点的入度则需要遍历全部的邻接表。因此，也有人采用逆邻接表的存储方式来加速求解给定顶点的入度。

   > 出度好求，入度的话要遍历来求。

5. 图的邻接表表示并不唯一，因此在每个定点对应的单链表中，各边结点的链接次序可以是任意的，它取决于建立邻接表的算法及边的输入次序。

### 十字链表

十字链表是有向图的一种链式存储结构。在十字链中，对应有向图中的每条弧有一个结点，对应于每个顶点也有一个结点。

#### 定义

![img](https://img.sped0nwen.com/image/2023/06/17/nnn9tp-0.webp)

弧结点中有5个域：尾域（tailvex）和头域（headvex）分别指示弧尾和弧头这个两个顶点在图中的位置；**链域hlink 指向弧头相同的下一条弧；链域tlink指向弧尾相同的下一条弧；**info域指向该弧的相关信息。这样，**弧头相同的弧就在同一个链表上，弧尾相同的弧也在同一个链表上**。

顶点结点中有3个域：data域存放顶点相关的数据信息，如顶点名称；firstin 和 firstout 两个域分别指向以该顶点为弧头或弧尾的第一个弧结点。

![img](https://img.sped0nwen.com/image/2023/06/17/npbtvh-0.webp)

图的十字链表存储结构定义如下：

```c
#define MAXVERTICENUM 100
typedef struct EdgeNode {
    int tailIndex, headIndex;
    struct EdgeNode *hLink, *tLink;
}EdgeNode;

typedef struct VerNode {
    Elemtype data;
    EdgeNode *arcIn, *arcOut;
}VerNode;

typedef struct {
    VNode xlist[MAXVERTICENUM];
    int VerNum,ArcNum; 
}OLGraph;
```

### 邻接多重表

邻接多重表是无向图的另一种链式存储结构。

在邻接表中，容易求得顶点和边的各种信息，但在邻接表中求两个顶点之间**是否存在边**而对**边指向删除**等操作时，需要分别在**两个顶点的边表中遍历**，效率低下。

#### 定义

与十字链表类似，在邻接多重表中，每条边用一个结点表示，其结构如下。

![img](https://img.sped0nwen.com/image/2023/06/17/nvttps-0.webp)

其中：

* mark为标志域，可用以标记该条边是否被搜索过；
* ivex 和 jvex 为该边依附的两个顶点在图中的位置；
* ilink指向下一条依附于顶点ivex的边；
* jlink指向下一条依附于顶点jvex的边；
* info为指向和边相关的各种信息的指针域。

每个顶点也用一个结点表示，它是由以下的两个域表示：

![img](https://img.sped0nwen.com/image/2023/06/17/nwkuhc-0.webp)

其中，data域存储该结点的相关信息，firstedge域指示第一条依附于该顶点的边。

在邻接多重表中，所有依附于同一顶点的边串联在同一链表中，由于每条边依附于两个顶点，因此每个边界点同时链接在两个链表中。

![img](https://img.sped0nwen.com/image/2023/06/17/nxosm7-0.webp)

#### 数据结构

图的基本操作是独立于图的存储结构的。而对于不同的存储方式，操作算法的具体实现会有不同的性能。

图的基本操作主要包括：

1. `Adjacent(G, x, y)`：判断图G是否存在边或（x，y）。

2. `Neighbors(G, x)`：列出图G中与结点x邻接的边。

3. `InsertVertex(G, x)`：在图中插入顶点x。

4. `DeleteVertex(G, x)`：在图中删除顶点x。

5. `AddEdge(G, x, y)`：若无向边（x，y）或有向边不存在，则向图G中添加该边

6. `RemoveEdge(G, x, y)`：若无向边（x，y）或有向边存在，则从图G中删除该边

7. `FirstNeighbor(G, x)`：求图G中顶点x的第一个邻接点，若有则返回顶点号。若x没有邻接点或图中不存在x，则返回 - 1

8. `NextNeighbor(G, x, y)`：假设图G中顶点y是顶点x的一个邻接点，返回出y之外顶点x的下一个邻接点的顶点号，若y是x的最后一个邻接点，则返回 -1 

9. `Get_edge_value(G, x, y)`：获取图G中边或（x，y）对应的权值

10. `Set_edge_value(G, x, y, v)`：设置图G中边或（x，y）对应的权值为v

11. 图的遍历算法：深度优先遍历和广度优先遍历
