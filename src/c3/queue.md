# 队列

## 队列的基本概念

### 定义

队列（Queue）。队列简称队。是一种操作受限的线性表，只允许在表的一端进行插入，而在表的另一端进行删除。向队列中插入元素称为入队或进队；删除元素称为出队或离队。其操作特性为先进先出（First In First Out，FIFO），并且只允许在队尾进，队头出。

![img](https://img.sped0nwen.com/image/2023/06/05/dbj1i8-0.webp)

* 队头（Front）：

  允许删除的一端，又称队首；

* 队尾（Rear）：

  允许插入的一端；

* 空队列：

  不包含任何元素的空表。

### 队列的基本操作

1. `InitQueue(&Q)`：初始化队列，构造一个空队列Q；

2. `QueueEmpty(Q)`：判断一个队列是否为空；

3. `EnQueue(&Q，x)`：入队，若队列未满，则将x加入使之成为新队尾；

4. `DeQueue(&Q，&x)`：出队，若队列非空，则将队首元素删除，并用x返回；

5. `GetHead(Q，&x)`：读队头元素，若栈顶元素非空，则用x返回栈顶元素。

## 队列的顺序存储

### 基本顺序队列的实现

队列的顺序实现市值分配一块连续的存储单元存放队列中的元素，并附设两个指针`front` 和 `rear` 分别指示队头元素和队尾元素的位置。**设队头指针指向队头元素，队尾指针指向队尾元素的下一个位置。**

![img](https://img.sped0nwen.com/image/2023/06/05/du2nsz-0.webp)

队列的顺序存储类型可以表示为：
write a `DesstroyQueue(&Q)` function for a queue, below is the data structure of the queue, after `DesstroyQueue(&Q)` is called, the queue should be destroy and no longer exist.
```c
#define MAXSIZE 100
typedef struct {
    Elemtype data[MAXSIZE];
    int front, rear;
} SqQueue;
```

* 初始状态（队列为空）：

  `Q.front == 0 && Q.rear == 0`

* 入队操作：

  队不满时，先送值到队尾元素，将队尾指针加1

* 出队操作：

  队非空时，先删除队头元素，再将队头指针加1

* 队列长度：

  `(Q.front + MAXSIZE - Q.rear)`

> **上溢现象：**
>
> 由上可知，`Q.front == 0 && Q.rear == 0` 可以作为队列的判空条件，但能否能用 `Q.rear == MAXSIZE` 作为队列已满的条件呢？显然是不行的，<u>因为队尾指针可能已经到了最尾端，但是队头指针可能不在初始位置</u>，而是在队列的中间，这时入队出现“上溢”，但这种溢出并不是真正的溢出，data数组中仍存在可以放置元素的位置，所以为“假溢出”。

### 循环顺序队列的实现

前面指出了队列的缺点，这里就引出循环队列的概念。<u>将队列臆造成一个环状的空间，即把存储队列元素的表从按逻辑上视为一个环</u>，称为循环队列。当队首指针 `Q.front == MAXSIZE - 1`，再前景一个位置就自动到0，这可以利用除法取余运算（%） 来实现。

![img](https://img.sped0nwen.com/image/2023/06/05/e0yclh-0.webp)

* 初始状态（队列为空）：

  `Q.front == 0 && Q.rear == 0`

* 入队操作：

  `Q.rear = (Q.rear + 1) % MAXSIZE`

* 出队操作：

  `Q.front = (Q.front + 1) % MAXSIZE`

* 队列长度：

  `(Q.front + MAXSIZE - Q.rear) % MAXSIZE`

> **循环队列如何判断队满还是队空？**
>
> 1. 一般做法：入队时少入对一个队列单元，约定<u>队尾指针的下一个标志是队头指针</u>作为队满的标志：
>
>    * 队列为空：
>
>      `Q.front == Q.rear`
>
>    * 队满操作：
>
>      `(Q.rear + 1) % MAXSIZE == Q.front`
>
>    * 队列长度：
>
>      `(Q.front + MAXSIZE - Q.rear) % MAXSIZE`
>
> 2. 在类型中添加表示成员个数的数据成员。队空时 `Q.size == 0`；队满时 `Q.size == MAXSIZE`。
>
> 3. 类型中添加`tag`数据成员，以区分是队满还是对空。
>
>    * `tag == 0` ，若因删除导致`Q.front == Q.rear` ，则为队空；
>    * `tag == 1`，若因添加导致`Q.front == Q.rear`，则为队满。
>

### 顺序队列的基本操作

#### 初始化

```c
void InitSqlQueue(SqlQueue &Q) {
    Q.front = 0;
    Q.rear = 0;
}
```

#### 判空

```c
bool SqQueueEmpty(SqQueue Q) {
    return Q.front == Q.rear;
}
```

#### 队列长度

```c
int SqQueueLength(SqQueue Q)
{
    return (Q.rear - Q.front + MAXSIZE) % MAXSIZE;
}
```

#### 队头元素

```c
bool GetSqQueueHead(SqQueue Q, Elemtype &e) {
    if (Q.rear == Q.front) {  // 队列为空，没有头元素
        return false
    }
    e = Q.data[Q.front];
    return true;
}
```

#### 入队

```c
bool EnSqQueue(SqQueue &Q, Elemtype e) {
    if ((Q.rear + 1) % MAXSIZE == Q.front) {   // 队列满，无法入队
        return false;
    }
    Q.data[Q.rear] = e;
    Q.rear = (Q.rear + 1) % MAXSIZE;           // 更新rear
    return true;
}
```

#### 出队

```c
bool DeSqQueue(SqQueue &Q, Elemtype &e) {
    if (Q.rear == Q.front) {                  // 队列空，无法出队
        return false;
    }
    e = Q.data[Q.front];
    Q.front = (Q.front + 1) % MAXSIZE;
    return true;
}
```

#### 清空队列

```c
bool ClearSqQueue(SqQueue &Q) {
    Q.rear = 0;
    Q.front = 0;
}
```

#### 销毁队列

```c
bool DestroySqQueue(SqQueue &Q) {
    Q.rear = 0;
    Q.front = 0;
    free(q);
}
```

#### 遍历队列

```c
bool TraverseSqQueue(SqQueue Q, bool (*visit)(Elemtype e))
{
    int i = Q.front;   // 创建一个新变量是因为这个i是需要自加的，我们不想在i自加进行遍历的时候影响Q的值
    while (i != Q.rear)
    {
        if (!visit(Q.data[i])) {
            return false;
        }
        i = (i + 1) % MAXSIZE;
    }
    return visit(Q.data[i]);
}
```

## 队列的链式存储结构

### 链式队列的实现

队列的链式存储又称为链队列，它实际上是一个<u>同时带有一个队头指针和队尾指针的单链表</u>。头指针指向队头结点，尾指针指向最后一个结点。

![image-20230605095444443](https://img.sped0nwen.com/image/2023/06/05/fsaikv-0.webp)

队列的链式存储类型可描述为：
```c
// QNode
typedef struct QNode{
    ElemType data;
    struct QNode* next;
}QNode;
// Queue
typedef struct{
    QNode *front, *rear;
}LinkQueue;
```

* 队列为空：

  `Q.front == NULL && Q.rear == NULL`

* 出队操作：

  若队列不空，则将队头元素删除，`Q.front`指向下一个结点；

* 入队操作：

  新建一个结点，将新结点插入到链队列的尾端，并让`Q.rear`指向这个新插入的结点。

> 不带头结点的链式队列在操作上往往比较麻烦，因此通常将链式队列设计成一个带头结点的单链表，这样插入和删除就统一了。

### 链式队列的基本操作

#### 初始化

```c
bool InitLinkQueue(LinkQueue &Q) {
    Q.front = Q.rear = (LinkNode*)malloc(sizeof(LinkNode));
    if (Q.front == NULL) {
        return false;
    }
    Q.front-> next = NULL;
    return true;
}
```

#### 判空

```c
bool EmptyLinkQueue(LinkQueue Q) {
    return Q.front == Q.rear;
}
```

#### 队列长度

```c
int LinkQueueLength(LinkQueue Q) {
    int length = 0;
    Qnode *f = queue -> front;
    while (f != NULL) {
        length += 1;
        f = f -> next;
    }
    return length;
}
```

#### 队头元素

```c
bool GetLinkQueueHead(LinkQueue Q, Elemtype &e) {
    if (Q.front == Q.rear) {    // 队列为空
        return false;
    }
    e = Q.front -> data;
    return true;
}
```

#### 入队

```c
bool EnLinkQueue(LinkQueue &Q, Elemtype e) {
    QNode *s = (QNode*)malloc(sizeof(struct QNode));
    if (s == NULL) {
        return false;
    }
    // 初始化s
    s -> data = e;
    s -> next = NULL;
    // 移值
    Q.rear -> next = s;
    Q.rear = s;
    return true;
}
```

#### 出队

```c
bool DeLinkQueue(LinkQueue &Q, Elemtype &e) {
    if (Q.front == Q.rear) {  // 栈空
        return false;
    }
    Qnode *old_front = Q.front;
    e = old_front -> data;
    // 更新Q.front
    Q.front = Q.front -> next;
    // 如果原先队里只有一个元素，出队里就是空了，要做处理
    if (Q.rear == old_front) {
        Q.rear = Q.front;
    }
    free(f);
    return true;
}
```

#### 清空队列

```c
void ClearLinkQueue(LinkQueue &Q) {
    QNode* p;
    while (Q.front != Q.rear) {
        p = Q.front -> next;
        Q.front -> next = p -> next;
        free(p);
    }
    // 空队列处理
    Q.rear = Q.front;
}
```

#### 销毁队列

```c
void DestroyLinkQueue(LinkQueue &Q) {
    QNode* p;
    while (Q.front != Q.rear) {
        p = Q.front -> next;
        Q.front -> next = p -> next;
        free(p);
    }
    // 销毁处理
    Q.rear = NULL;
    Q.front = NULL;
}
```

#### 遍历队列

```c
bool TraverseLinkQueue(LinkQueue Q, bool(*visit)(ElemType e)){
    QNode *p = Q.front -> next;
    while (p != NULL) {
        if (!visit(p->data))  // call visit function on each element
            return false;
        p = p->next;    // point to next element
    }
    return true;    // traversal successful
}
```
