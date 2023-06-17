# 双链表

<!-- toc -->

单链表结点中只有一个只指向后继的指针，使得单链表只能从头结点开始一次顺序的先后遍历。要访问某个结点的前驱结点（插入删除操作时），只能从头开始遍历，访问后继节点的时间复杂度为O(1)，访问前驱结点的时间复杂度为O(n)。

为了克服单链表的上述缺点，引入了双链表，双链表结点中有两个指针`prior/prev`和`next`，分别指向其前驱结点和后继结点。

## 定义

双链表中结点类型描述：

![img](https://img.sped0nwen.com/image/2023/06/02/inrwsn-0.webp)

```c
typedef struct DNode{
    ElemType data;
    struct DNode *prev, *next;
}DNode, *DLinkList;
```

在双链表中，按位查找和按值查找的操作和单链表中的相同（遍历）。<u>但在插入和删除上有着较大的不同</u>。因为双链表有`prior/prev`指针，可以很方便地找到其前驱结点，因此插入，删除结点的时间复杂度为O(1)。

## 双链表上的基本操作

### 初始化

```c
bool InitDoubleLinkedList(DLinkList& L) {
	D = (struct Node *)malloc(sizeof(DNode)); // 生成一个头结点
	if (D == NULL) {                          // 内存不足，分配失败
		return false;
	}
	D -> prev = NULL;                         // 头结点之后暂时没有任何结点
	D -> next = NULL;
	return true;
}
```

### 前插操作

![img](https://img.sped0nwen.com/image/2023/06/02/j055gm-0.webp)

将结点s插入到指针p所指结点b之前：

```c
bool DLinkList_Insert(DLinkList& L,ElemTyep c){
    DNode *back;
    front = back -> prev;
    /* 先检查位置是否合法...  */
    DNode *s = (DNode*)malloc(sizeof(DNode));
    s -> data = c;
    //第一步：结点s的prior指针指向左边的结点
    s -> prev = front;    
    //第二步：结点b的前驱结点（也就是左边的结点）的next指针指向结点s
    front -> next = s;
    //第三步：结点s的next指针指向结点b
    s -> next = back;
    //第四步：结点b的prior指针指向结点s
    back -> prev = s;        
    return true;
}
```

> 注意：
>
> 第一，二步必须在第四步之前，否则`*p`的前驱结点的指针就会丢失，导致插入失败。

### 后插操作

将结点s 插入到指针p所指的节点b 之后（如上图，假设后面的结点是d）：

```c
bool DLinkList_Insert(DLinkList& L,ElemType c){
    DNode *front;
  	back = front -> next;
    DNode *s = (DNode*)malloc(sizeof(DNode));
    s -> data = c;
    //第一步：将结点s 的next指针指向后继结点
    s -> next = back;
    //第二步；将后继结点的prior指针指向结点s
    back -> prev = s;
    //第三步：将结点s的prior指针指向结点b
    s -> prev = front;
    //第四步：将结点b的next指针指向结点s
    front-> next = s;
    return true;    
}
```

同样的，前插操作和后插其实没区别，注意第四步即可。

### 删除操作

![img](https://img.sped0nwen.com/image/2023/06/02/jwh50x-0.webp)

删除指针p指向的结点b：

```c
bool DLinkList_Delete(DLinkList& L){
    DNode *p;
  	front = p -> prev;
  	back = p -> next; 
    //第一步：将结点b的前驱结点的next指针指向结点c
    front -> next = back;
    //第二步：将结点c的prior指针指向结点b的前驱结点
    back -> prior = front;
    //释放结点空间
    free(p);
}
```

如果是删除指针p指向的结点的后继结点，也就是后删：

```c
bool DLinkList_Delete(DLinkList& L){
    DNode *p;
    back = p -> next;
    p -> next = back -> next;
    back_of_the_back = back -> next;
    back_of_the_back -> prior = p;
    free(q);
}
```

### 逆转
```c
void Double_Linked_List_Reverse(DLinkList& L) {
    DNode *curr = L;
    DNode *prev = NULL;
    DNode *next = NULL;
    while (cur != NULL) {
    	next = curr -> next;    // 暂存next
        curr -> next = prev;    // 拿prev取代next，为什么不拿curr -> prev是因为需要空头结点
        curr -> prev = next;    // 拿next替代prev
        // 变量移位
        prev = current;
        current = next;
    }
}
```

### 其他操作

继承自单链表

* [序号索引查找](./linked_list.md#序号索引查找)
* [按值查找](./linked_list.md#按值查找)
* [求表长](./linked_list.md#求表长)
* [显示所有结点信息](./linked_list.md#显示链表所有结点信息)
* [清空整个链表](./linked_list.md#清空整个链表)

