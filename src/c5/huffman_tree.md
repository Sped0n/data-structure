# 哈夫曼树

<!-- toc -->

## 哈夫曼树的定义

在许多实际应用中，树中结点常常被赋予一个表示某种意义的数值，称为该**结点的权**。从树根结点到任意结点的路径长度（经过的边数）与该结点上权值的乘积，称为该结点的**带权路径长度**。树中所有结点的带权路径长度之和称为全树的带权路径长度，记为：
$$
\mathrm{WPL=\sum_{i=1}^{n} W_i L_i}
$$

$\mathrm{W_i}$ 是第$\mathrm i$个结点所带的权值，$\mathrm{L_i}$ 是该结点到根结点的路径长度。

在含有n个带权叶子的二叉树中，其中带权路径长度（WPL）最小的二叉树称为哈夫曼树，也称最优二叉树。

![img](https://img.sped0nwen.com/image/2023/06/13/xlbhlc-0.webp)

(a)  WPL = 7 x 2 + 5 x 2 + 2 x 2 + 4 x 2 = 36；

(b)  WPL = 7 x 3 + 5 x 3 + 2 x 1 + 4 x 2 = 46；

(c)  WPL = 7 x 1 + 5 x 2 + 2 x 3 + 4 x 3 = 35；

其中，c中的树的WPL最小，可以验证，它恰好为哈夫曼树。

## 哈夫曼树的构造

算法描述如下：

1. 将这n个结点分别作为n棵仅含有一个结点的二叉树，构成森林F。
2. 构造一个新结点，从F中选取两颗树节点权值最小的树作为新结点的左、右子树，并且将新节点的权值置为左、右子树上根结点的权值之和。
3. 从F中删除刚选出的两棵树，同时将新得到的树加入到F中。
4. 重复步骤2和3，直至F中只剩下一棵树为止。

![img](https://img.sped0nwen.com/image/2023/06/13/xqprbe-0.webp)

从上述构造过程中可以看出哈夫曼树具有如下特点：

1. 每个初始结点最终都成为叶结点，且权值越小的节点到根结点的路径长度越大。
2. 构造过程中共新建了 n-1 个结点，因此哈夫曼树中的结点总数为 2n - 1。（合并）
3. 每次构造都选择 2 棵树作为新结点的孩子，因此哈夫曼树中不存在度为 1 的结点。

### 哈夫曼编码

对应待处理的一个字符串序列，若对每个字符用同样长度的二进制表示，则称这种编码方式为**固定长度编码**。若允许对不同字符用不等长的二进制位表示，则这种方式称为**可变长度编码**。 可变长编码的特点是对高频率的字符赋以端编码，而对频率较低的字符则以较长的一些的编码，从而可以使字符平均编码长度剪短，起到压缩数据的效果。

若没有一个编码是另一个编码的前缀，则这样的编码为前缀编码。

![img](https://img.sped0nwen.com/image/2023/06/13/xrgybp-0.webp)

构造出的哈夫曼树不唯一，但各哈夫曼树的带权路径长度相同且为最优。
