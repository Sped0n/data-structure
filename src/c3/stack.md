# 栈

<!-- toc -->

## 栈的基本概念

### 定义

只允许在一端进行插入或删除操作的线性表。首先，栈是一种线性表，但限定这种线性表只能在某一段进行插入和删除操作。（<u>类似一叠盘子，放和取都在盘子堆的最顶端</u>）

* 栈顶（Top）：

  线性表允许进行插入和删除的一端。

* 栈底（Bottom）：

  固定的，不允许进行插入和删除的另一端。

* 空栈：

  不含任何元素。

![img](https://img.sped0nwen.com/image/2023/06/03/xojcwd-0.webp)

> 如上图：a1为栈底元素，an为栈顶元素。由于栈只能在栈顶进行插入和删除操作，故进栈次序依次为a1，a2，... ,an而出栈次序为an，...，a2，a1。栈的明显的操作特征为后进先出（Last In First Out，LIFO），故又称后进先出的线性表。

### 栈的基本概念

1. `InitStack(&S)`：初始化空栈S；
1. `StackEmpty(S)`：判断一个栈是否为空；
1. `Push(&S，x)`：进栈，若栈未满，则将x加入使之成为新栈顶；
1. `Pop(&S，&x)`：出栈，若栈非空，则将栈顶元素，并用x返回；
1. `GetTop(S，&x)`：读栈顶元素，若栈顶元素非空，则用x返回栈顶元素；
1. `DestroyStack(&S)`：销毁栈，并释放栈S占用的存储空间。

## 栈的顺序存储结构

### 顺序栈的实现

采用顺序存储的栈称为**顺序栈**，它是利用一组地址连续的存储单元存放自栈底到栈顶的数据元素，同时附设一个指针（top）指示当前栈顶的位置。

栈的顺序存储类型可以用以下表示：

```c
#define MAXSIZE 100          // 栈中元素的最大值
typedef struct {
    Elemtype data[MAXSIZE];  // 存放栈中元素
    int top;                 // 栈顶元素索引
} SqStack;
```

* 栈顶指针：

  `S.top`，初始时设置`S.top = -1`（栈空的时候为-1，类似一个无效索引）；栈顶元素：`S.data[S.top]`；

* 进栈操作：

  栈不满时，<u>先</u>栈指针加1，<u>再</u>送值到栈顶元素；

  > 这个先后顺序参考[下面的程序](#入栈)就能明白，具体说就是如果先送值，这个时候的索引还没加一，是无效的。

* 出栈操作：

  栈非空时，<u>先</u>去栈顶元素值，再将栈顶指针减1；

  > 和原理上面相同，参考[下面的程序](#出栈)

* 栈长：

  `S.top + 1`

* 栈空和栈满的判断：

  * 栈空：`S.top == -1`
  * 栈满：`S.top == MAXSIZE - 1`

### 顺序栈的基本操作

![img](https://img.sped0nwen.com/image/2023/06/04/sy2n4y-0.webp)

#### 初始化

```c
bool InitStack(SqStack &S, int maxsize=50) {
    S.data = new Elemtype[maxsize];
    if (S.data == NULL) {
        return false;
    }
    S.top = -1;     // 初始化栈顶索引
    return true;
}
```

#### 判空

```c
bool StackEmpty(SqStack S) {
    if (S.top == -1) {
        return true;
    }
    return false;
}
```

> 这里没做操作，所以不需要引用。

#### 入栈

```c
bool Push(SqStack &S, Elemtype e) {
    if (S.top == MAXSIZE - 1) {      // 栈满，报错
        return false;
    }
    S.top += 1;                      // 栈顶索引更新
    S.data[S.top] = e;               // 将元素放入栈顶
    return true;
}
```

#### 出栈

```c
Elemtype Pop(SqStack &S) {
    if (S.top == -1) {               // 栈空，报错
        return false;
    }
    Elemtype temp = S.data[S.top];
    S.top -= 1;
    return temp;
}
```

> 这里函数的输入参数为什么是&呢，因为如果这里没有&引用变量e就会发生所有权转移（从主函数到本函数），变量e在函数结束之后就会被销毁，从而让我们这个变量e在主函数无法使用。

#### 读栈顶操作

```c
bool GetTop(SqStack &S, Elemtype &e) {
    if (S.top == -1) {                // 栈空，报错
        return false;
    }
    e = S.data[S.top];
    return true;
}
```

## 栈的链式存储结构

采用链式存储的栈称为**链栈**，链栈的优点是便于多个栈共享存储空间和提高其效率，且不存在栈满上溢的情况。通常采用<u>单链表</u>实现，并规定<u>所有操作都是在单链表的表头</u>进行的。这里规定<u>链栈没有头结点</u>，`top`指向栈顶元素。

![img](https://img.sped0nwen.com/image/2023/06/04/u7wwsl-0.webp)

### 链栈的实现

```c
typedef struct SNode{
    ElemType data;
    struct LinkNode* next;
}SNode, *LinkStack;
```

### 链栈的基本操作

#### 初始化

```c
LinkStack InitLinkStack() {
    LinkStack S = (LinkStack)malloc(sizeof(struct SNode));
    S -> next = NULL;
    return S;
}
```

#### 判空

```c
bool LinkStackEmpty(LinkStack &S) {
    return S -> next == NULL;
}
```

#### 计算链栈长度

```c
int LinkStackLength(LinkStack &S) {
    SNode *p = S -> next;
    int len = 0;
    while (p != NULL) {
        p = p -> next;
        len++;
    }
}
```

#### 入栈

```c
bool Push(LinkStack &S, Elemtype e) {
    SNode *NewNode = (SNode*)malloc(sizeof(struct SnNode));
    if （NewNode == NULL) {
        return false;        // 结点分配失败
    }
    NewNode -> data = e;
    NewNode -> next = S;
    S = NewNode
}
```

#### 出栈

```c
Elemtype Pop(LinkStack &S) {
    LinkStack *temp = S;
    S = S -> next;
    return temp -> data;
}
```

#### 读栈顶操作

```c
bool GetTop(LinkStack &S, Elemtype &e) {
    if (S -> next == NULL) {
        return false;
    }
    e = S -> data;
    return true;
}
```

