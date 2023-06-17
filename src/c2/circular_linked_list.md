# 循环链表

<!-- toc -->

## 循环单链表

### 定义

循环单链表和单链表的区别在于：循环单链表的最后一个结点的指针不是NULL，而改为指向头结点，从而整个链表形成一个环。

![img](https://img.sped0nwen.com/image/2023/06/03/ns7r8v-0.webp)

在循环单链表中，表尾结点`tail`的`next`指针指向头结点，所以表中没有指针域为`NULL`的结点，因此，循环单链表的判空条件不是头结点的指针是否为空，而是判断头结点的指针是否等于头指针，就是说是否指向自身。

### 循环单链表上的基本操作

#### 初始化（默认带头结点）

```c
LinkList InitLinkList(){
    LNode *L = (LNode*)malloc(sizeof(LNode));
    if(L == NULL){
        return false;
    }
    L->next = L; // 和单链表的区别就在这里，next指向了自己
    return L;
}
```

#### 判空

```c
bool IsEmpty(LinkList &L) {
    if(L->next == L){
        return true;
    }
    return false;
}
```

#### 判断是否为表尾结点

```c
bool IsTail(LinkList &L, LNode *p){
    if(p->next == L){     // 逻辑：表尾结点的下一个结点是头结点
        return true;
    }
    return false;
}
```

有时对单链表常做的操作是在表头表尾操作的，此时对循环单链表不设头指针而仅设尾指针，从而使得操作效率更高。原因是若设头指针，对表尾进行操作的时间复杂度为O(n)，而设尾指针`tail`，`tail->next`即为头指针，对于表头表尾进行操作时都只需要O(1)。

#### 逆转

```c
void Circular_Linked_List_Reverse(LinkList &L){
    // L为旧表的表头，新表的表尾
    LNode *curr = L;
    LNode *prev = NULL;
    LNode *next = NULL;
    while (curr != NULL) {
        next = curr -> next;
        curr -> next = prev;
        prev = curr;
        curr = next;
    }
    // prev是最后一个可用的curr，所以这里用prev
    L -> next = prev;
}
```



#### 其他操作

均参考单链表：

* [结构定义](./linked_list.md#定义)
* [建立单链表](./linked_list.md#建立单链表)
* [序号索引查找](./linked_list.md#序号索引查找)
* [按值查找](./linked_list.md#按值查找)
* [插入结点](./linked_list.md#插入结点)
* [删除结点](./linked_list.md#删除结点)
* [显示所有结点信息](./linked_list.md#显示链表所有结点信息)
* [求表长](./linked_list.md#求表长)
* [显示链表所有结点信息](./linked_list.md#显示链表所有结点信息)

## 循环双链表

### 定义

知道了循环单链表，那循环双链表理解起来就很简单了。循环双链表的表尾结点的`next`指针指向头结点，同时头结点的`prev`指针指向表尾结点。

![img](https://img.sped0nwen.com/image/2023/06/03/nxt9vi-0.webp)

### 循环双链表上的基本操作

#### 初始化

```c
bool InitDLinkList(DLinkList &L){
    L = (DNode*)malloc(sizeof(DNode));
    if (L == NULL) {
        return false;
    }
    L -> prior = L;
    L -> next = L;
    return true;
}
```

#### 判空

```c
bool IsEmpty(DLinkList &L){
    if (L -> next == L && L -> prior == L) {
        return true;
    }
    return false;
}
```

#### 判断是否为表尾结点

```c
bool IsTail(LinkList &L, LNode *p){
    if (p -> next == L) {
        return true;
    }
    return false;
}
```

#### 逆转

基于[双链表的逆转操作](./double_linked_list.md#逆转)

```c
void Circular_Double_Linked_List_Reverse(DLinkList& L) {
    // L为旧表的表头，新表的表尾
    DNode *curr = L;
    DNode *prev = NULL;
    DNode *next = NULL;
    while (cur != NULL) {
    	next = curr -> next;
        curr -> next = prev;
        curr -> prev = next;
        prev = current;
        current = next;
    }
    // prev是最后一个可用的curr，所以这里用prev
    prev -> prev = L;
    L -> next = prev;
}
```

#### 其他操作

* [定义](./double_linked_list.md#定义)
* [初始化](./double_linked_list.md#初始化)
* [前插操作](./double_linked_list.md#前插操作)
* [后插操作](./double_linked_list.md#后插操作)
* [删除操作](./double_linked_list.md#删除操作)

## 静态链表

### 定义

静态链表借助数组来描述线性表的链式存储结构，结点也有数据域`data`和指针域`next`，与链表中的指针不同，这里的指针是结点的相对地址（数组下标），又称游标。静态链表也要预先分配分配一块连续的内存空间。

![img](https://img.sped0nwen.com/image/2023/06/03/otv1q3-0.webp)

静态链表结构类型的描述如下：

```c
#define MAXSIZE 100
typedef struct{
    ElemType data;
    int next;
}SLinkList[MAXSIZE];
```

静态链表以`next == -1`作为其结束的标志。静态链表的插入，删除操作与动态链表的相同，只需要修改指针，而不需要移动元素。
