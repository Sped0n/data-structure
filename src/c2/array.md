# 顺序表

<!-- toc -->

线性表的顺序存储又称顺序表。表中元素的逻辑顺序与其物理顺序相同。

## 定义

### 静态定义

```c
#define MAXSIZE 50;                    //定义线性表的最大长度
typedef struct {
    ElemType data[MAXSIZE];            //顺序表的元素
    int length;                        //顺序表的当前长度
}SqList;
```

静态分配时，数据量小的话，会造成数组空间的浪费（因为这是一种预分配的方式！！！）；而如果是数据量大的话，就会造成溢出，进而导致程序崩溃。所以我们采用下面更灵活的方式来。

### 动态定义

```c
#define InitSize 100                //定义线性表的初始长度
typedef struct {                    
    ElemType* data;                 //指示动态分配数组的指针
    int MAXSIZE,length;             //数组的最大容量和当前个数
}SqList;
```

* C的初始化分配语句为：

  ```c
  L.data = (ElemType*)malloc(sizeof(ElemType)*InitSize);
  ```
  这句话的意思是malloc向系统申请分配``sizeof(ElemType)*InitSize`的内存空间，`ElemType`类型的指针指向这块内存的首地址。

* C++的初始化分配语句为：

  ```cpp
  L.data = new ElemType[InitSize];
  ```

## 顺序表上的基本操作

### 初始化（静态）

```c
//初始化顺序表(静态分配)
bool InitSqList(SqList &L) {
    for (int i = 0; i < MAXSIZE; i++) {
        L.data[i] = 0;
    }
  	// MAXSIZE跑完顺序表还有数据，则溢出报错
    if (!L.data)
        exit(OVERFLOW);
    L.length = 0;
    return true;
}
```

### 初始化（动态）

```c
//初始化顺序表
void InitSeqList(SqList &L) {
    L.data = (ElemType*)malloc(sizeof(ElemType) * InitSize);
    //L.data = new ElemType[InitSize];   //C++动态分配
    L.length = 0;
    L.MaxSize = InitSize;
}
```

### 数组扩充（动态）

```c
//增加动态数组的长度
void IncreaseSize(SqList &L, int len) {
    ElemType* p = L.data;                      //新建指针p指向数组首部,也就是p指针和data指针同时指向数组首位元素
    L.data = (ElemType*)malloc(sizeof(ElemType) * (L.MAXSIZE + len));  //在别处开辟(MAXSIZE+len)*sizeof(ElemType)连续的空间，  并将data指针指向这片空间的首地址
    for (int i = 0; i < L.length; i++) {
        L.data[i] = p[i];                    //将数据复制到区域
    }
    L.MAXSIZE += len;
    free(p);
}
```

### 求表长

```c
int Length(SqList L) {
    return L.length;
}
```

### 按值查找

```c
int LocateElem(SqList L, ElemType e) {
    for (int i = 0; i < L.length; i++) {
        if (e == L[i]) {
            return i + 1;
        }
    }
    return -1;
}
```

**算法复杂度分析**

* 最好情况：查找的元素就在表头，时间复杂度为O(1)

* 最坏情况：查找的元素在表尾，时间复杂度为O(n)

* 平均情况：比较的平均次数为 (n+1) / 2

因此，线性表的按值查找操作算法时间复杂度为O(n)。

### 插入操作

```c
bool ListInsert(SqList &L,int i,ElemType e) {
    if( i < 1 || i > L.length + 1){              //判断输入的i是否符合范围
        return false;
    }
    if( L.length > MAXSIZE){                    //如果存储空间已满，则插入失败
        return false;
    }
    for( int i = L.length; i >= i; i--){        //将第i个元素及之后的元素后移
        L.data[i] = L.data[i-1];
    }
    L.data[i] = e;
    L.length ++;
    return true;    
}
```

**算法复杂度分析**

* 最好情况：在表尾插入，时间复杂度为O(1)

* 最坏情况：在表头插入，时间复杂度为O(n)

* 平均情况：所需移动结点的平均次数为 n/2

因此，线性表的插入操作算法时间复杂度为O(n)。

### 删除操作

```c
bool ListDelete(SqList &L,int i,ElemType &e) {   //删除第i位上的数，并通过e返回其值
    if( i < 1 || i > L.length){                  //判断输入的i是否符合范围
        return false;
    }
    e = L.data[i];
    for( int i = i; i < L.length; i++){
        L.data[i-1] = L.data[i];
    }
    L.length --;
    return true;    
}
```

**算法复杂度分析**

最好情况：在表尾删除，时间复杂度为O(1)

最坏情况：在表头删除，时间复杂度为O(n)

平均情况：所需移动结点的平均次数为 (n-1) / 2

因此，线性表的删除操作算法时间复杂度为O(n)。

