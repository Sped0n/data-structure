# 单链表

由于顺序表的插入、删除操作需要移动大量的元素，影响运行效率，由此引进了线性表的链式存储。链式存储线性表时，不需要使用地址连续的存储单元，对线性表的插入删除不需要移动元素，而只需要修改指针。

## 定义

每个链表结点，除了存放元素自身的信息外，还需要存放一个指向后继的指针。

```c
typedef struct LNode{
    ElemType data;
    struct LNode* next;
}LNode, *LinkList;          // LNode,LinkList 其实是一样的，只是表示结点和链表更方便
```

## 单链表上的基本操作

### 初始化

#### 不带头结点的单链表

```c
bool InitLinklist(LinkList& L) {
    L = nullptr;
    return true;
}
```

#### 带头结点的单链表

```c
bool InitLinkList(LinkList& L){
    L = (Lnode*)malloc(sizeof(LNode));    // 生成一个头结点
    if( L == nullptr){                    // 内存不足，分配失败
        return false;
    }
    L -> next = nullptr;                    // 头结点之后暂时没有任何结点
    return true;
}
```

**头结点和头指针的区别**：

不管带不带头结点，头指针始终指向链表的第一个结点，而头结点是带头结点的链表中的第一个结点，结点内通常不存储信息。

**引入头结点的优点**：

1. 结点操作一致性：

   由于头结点的位置被存放在头结点的指针域中，所以在链表的第一个位置上的操作和在链表的其他位置上的操作一致，无需进行特殊处理。

2. 空表非空表处理一致性：

   无论链表是否为空，其头指针都指向头结点的非空指针（空表中头指针的指针域为空），因此空表和非空表的处理也就得到了统一。

### 建立单链表

#### 头插法

![img](https://img.sped0nwen.com/image/2023/06/01/sv1utb-0.webp)

```c
// 请配图食用！！！
LinkList List_HeadInsert(LinkList& L, int n){
    L = (LinkList)malloc(sizeof(LNode));
    L -> next = nullptr;                              // 创建一个带头结点的空链表
    for (int i = 0; i < n; i++) {
        LNode* s = (LNode*)malloc(sizeof(LNode));   // 新建一个结点
        cin >> s-> data;

        s -> next = L -> next;                          // 头结点后插入新结点
        L -> next = s;        
    }
    return L;
}
```

头插法建立的链表时，读入数据的顺序与生成的链表数据是相反的，总的时间复杂度为O(n)。

#### 尾插法

![img](https://img.sped0nwen.com/image/2023/06/01/tw38tp-0.webp)

在尾插法的实现过程中，要创建一个链表尾指针，标注链表的最后一个节点，这样就与新加入的结点联系起来，方便操作。

```c
LinkList List_TailInsert(ListLink& L, int n){
    L = (LinkList)malloc(sizeof(LNode));
    L -> next = nullptr;
    LNode* tail = (LNode*)malloc(sizeof(LNode));  // 生成表尾指针
    tail = L;            //表尾指针指向头结点
    for( int i = 0; i < n; i++){
        LNode* s = (LNode*)malloc(sizeof(LNode)); // 新建结点
        cin >> s->data;
        tail -> next = s;                         // 将尾指针的下一个结点指向新建结点
        tail = s;                                 // 将新建结点设为表尾
    }
    tail -> next = nullptr;                       // 尾指针后继设为空
    reutrn L;    
}
```

尾插法建立链表时，读入数据的顺序和生成的链表数据是相反的，中的时间复杂度为O(n)。

### 序号索引查找

```c
LNode* GetElem(LinkList L,int i){
    int count = 1;
    LNode* p = L -> next;         // 指针p指向头结点
    if( i == 0){                // 相当于返回头结点
        return L;
    }
    if( i < 1){                        
        return nullptr;          // i无效，返回空    
    }
    while( p && count < i){
        p = p -> next;
        count++;
    }
    return p;
}
```

时间复杂度为O(n)。

### 按值查找

```c
LNode* LocateElem(LinkList L, ElemType e){
    LNode* p = L -> next;                    // 指针p指向头结点
    while( p != nullptr && p->data != e){  // 遍历匹配
        p = p -> next;
    }
    return p;
}
```

时间复杂度为O(n)。

### 插入结点

#### 后插

![img](https://img.sped0nwen.com/image/2023/06/02/hftzmx-0.webp)

```c
bool ListLNode_Insert(LinkList& L, int i, ElemType e){
    LNode* p = GetElem(L, i-1);                //找到插入位置的前驱结点
    LNode* s = (LNode*)malloc(sizeof(LNode));
    s -> data = e;
    s -> next = p -> next;
    p -> next = s;
    return true;
}
```

该算法主要的时间花费在寻找第i-1个结点上，时间复杂度为O(n)，若在给定的结点后插入新结点，则时间复杂度为O(1)。

#### 前插

```c
bool ListLNode_ForwardInsert(LinkList& L, int i, ElemType e){
    LNode* p = GetElem(L,i);                  // 找到第i个元素
    LNode* s = (LNode*)malloc(sizeof(LNode)); // 分配新数组的空间
    s -> data = e;
    s -> next = p->next;
    p -> next = s;
    swap(s->data,p->data);                    // 数据域交换
    return true;    
}
```

其实就是在该节点后面插入一个新结点，再将两个结点的数据域交换，就轻松的完成了后插。

### 删除结点

#### 不清空结点内容

```c
bool ListLNode_Delete(LinkList& L, int i){
    LNode* p = GeElem(L, i - 1);            // 找到第i-1个元素,即*q的前驱结点
    LNode* q = p -> next;
    p -> next = q -> next;
    free(q);
    return true;
}
```

<u>找到需要删除结点的前一个结点</u>，将这个前向结点指向删除结点的下一个结点，相当于<u>跳过</u>这个删除结点，但并没有清空内容。

#### 清空结点内容

```c
bool ListLNode_Complete_Delete(LinkList& L, int i){
    LNode* p = GeElem(L, i);            // 找到第i-1个元素,即*q的前驱结点
    LNode* q = p -> next;
    p -> data = q -> data;
    p -> next = q -> next;
    free(q);
    return true;
}
```

<u>找到需要删除的结点</u>，将其的内容和指向改为下一个元素，相当于用下一个结点<u>替代</u>这个删除结点，清空了内容。

### 求表长

```c
int LinkListLength(LinkList L){
    LNode* p = L -> next;
    int len = 1;
    while( p != nullptr){ // 遍历并且自加累积器
        p = p -> next;
        len++;
    }
    return len;    
}
```

### 显示链表所有结点信息

```c
void List_Display(LinkList L){
    LNode* p = L -> next;
    while( p != nullptr){
        cout << p -> data << " ";
        p = p -> next;
    }
    cout << endl;
}
```

### 逆转链表

#### 双指针法

创建两个指针，`cur`和`pre`，`pre` 在右，`cur`在左。每次让`pre`的`next`指针指向`cur`，完成一次局部反转，之后两个指针同时向右移，直至链表尾。

![img](https://img.sped0nwen.com/image/2023/06/02/ijox5n-0.webp)

```c
void Linked_List_Reverse(LinkList& L){
    LNode *curr = L;
    LNode *prev = NULL;
    LNode *next = NULL;
    while (curr != NULL) {
        next = curr -> next;
        curr -> next = prev;
        prev = curr;
        curr = next;
    }
}
```

