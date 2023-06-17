# 基本概念

<!-- toc -->

## 定义

图G由顶点集V和边集E组成，记为 $G = (V, E)$，其中$V(G)$表示图G中顶点的有限非空集；$E(G)$表示图G中顶点之间的关系（边）集合。若 $V = \{v1,v2,...,vn\}$，则用$|V|$表示图G中顶点的个数，也称图G的阶。
$$
E=\{(u,v)\ |\ u\in V,\ v\in V\}
$$
用$|E|$表示图G中的边数。

## 名词

### 有向图

若 E 是有向边（弧）的有限集合时，则图G为有向图。弧是顶点的有序对，记为，其中v，w是顶点，v称为**弧尾**，w称为**弧头**， 称为**顶点v到w的弧**，也称v邻接到w，或w邻接自v。

![img](https://img.sped0nwen.com/image/2023/06/17/i7mtxq-0.jpg)

### 无向图

若 E 是无向边（简称边）的有限集合时，则图G为无向图。便是顶点的无序对，记为（v，w）或（w，v），因为（v，w）= （w，v），其中v，w是顶点。可以说顶点w和顶点v互为邻接点。边（v，w）依附顶点w，v，或者说边（v，w）和顶点v，w相关联。

![img](https://img.sped0nwen.com/image/2023/06/17/i7tnzf-0.webp)

### 简单图

一个图若满足：

1. 不存在重复边（不平行）
2. 不存在顶点到自身的边，则称图G为简单图（不自环）

这个图为简单图

### 多重图

简单图的相反，允许平行，允许自环

### 完全图（简单完全图）

#### 无向完全图

在无向图中，若任意两个顶点之间都存在边，则称该图为无向完全图。

**边数**：
$$
\frac{n(n-1)}{2}
$$


#### 有向完全图

若任意两个顶点之间都存在<u>方向相反的两条弧</u>，则称该图为有向完全图。

**边数**：
$$
n(n-1)
$$

### 子图

设有两个图 $G = (V,E)$和 $G^\prime = (V^\prime，E^\prime)$,若 $V^\prime$ 是 $V$的子集，且 $E^\prime$ 是 $E$ 的子集，则称 $G^\prime$ 是 $G$ 的子集。若有满足$V(G^\prime)= V(G)$的子图$G^\prime$，则称其为$G$的**生成子图**。

### 连通、连通图和连通分量

在无向图中，若从顶点v到顶点w有路径存在，则称v和w是连通的。**若图G中任意两个顶点都是连通的，则称图G为连通图**，否则称为非连通图。**无向图中的极大连通子图称为连通分量**。若有一个图中有 n 个顶点，并且边数小于 n - 1，则此图必是非连通图。**无向图中的极大连通子图称为连通分量。**若一个图有n个顶点，并且边数小于n-1，则此图必是非连通图。

> **极大连通子图**是无向图的连通分量， 极大即要求该连通子图包含所有的边；
>
> **极小连通子图**是 即要保持图连通又要使得边数最少的子图。

![img](https://img.sped0nwen.com/image/2023/06/17/itrdbm-0.webp)

### 强连通图、强连通分量

在**有向图**中，**若从顶点 v 到顶点 w 和从顶点w 到 顶点 v 之间都有路径**，则称这两个顶点是**强连通**的。若图中任何一对顶点都是强连通的，则称此图为**强连通图**。 有向图中的极大强连通图称为有向图的**强连通分量**。

### 生成树、生成森林

连通图的生成树是包含图中全部顶点的一个极小连通子图。若图中顶点数为$n$，则它的生成树含有 $n - 1$条边。对生成树而言，若砍去它的一条边，则会变成非连通图，若加上一条边则会形成一个回路。在非连通图中，连通分量的生成树构成了非连通图的生成森林。

### 顶点的度，入度和出度

图中每个顶点的度定义为以该顶点为一个端点的边的个数。（<u>树的结点的度为子结点个数</u> ）

对于无向图，顶点$v$的度是指依附于该顶点的边的条数。具有$n$个顶点，$e$条边的无向图中，无向图全部顶点的度的和等于边数的两倍。

对于有向图，顶点的度分为<u>入度和出度</u>；入度是以顶点为终点的有向边的数目，而出度是以顶点为起点的有向边的数目；顶点的度等于其入度和出度之和。 在具有$n$个顶点、$e$条边的有向图中，有向图的全部顶点的入度之和与出度之和相等，并且等于边数。

### 边的权和网

一个图中，每个边都可以标上具有某种含义的数值，该数值称为边的权值。这种边上带有权值的图为带权图，也称网。

### 稠密图，稀疏图

边数很少的图称为稀疏图，反之称为稠密图。稀疏和稠密是相对的。

一般认为图的$|E| < |V| \log |V| $时，可以将图视为稀疏图。

### 路径、路径长度和 回路

顶点$V_p$到顶点$V_q$之间的一条路径是指顶点序列$V_p，V_i，\cdots ，V_q$。路径上边的数目称为**路径长度**。第一个顶点和最后一个顶点相同的路径称为**回路或环**。若一个图有$n$个顶点，并且有大于 $n - 1$ 条边，则此图一定有环。

### 简单路径，简单回路

在路径序列中，顶点不出现重复的路径称为**简单路径**。除第一个顶点和最后一个顶点外，其余顶点不重复出现的回路称为**简单回路**。

### 距离

从顶点U 出发到顶点V的最短路径若存在，则此路径的长度称为 从 U 到 V的距离。若从U 到 V根本不存在路径，则记该距离为无穷。

### 有向树

一个顶点的入度为0，其余顶点的入度均为1的有向图，称为有向树。