---
title: 数据结构-队列（Queue）学习笔记
date: 2020-03-09 10:29:13
tags:
	- 数据结构
	- 学习笔记
categories: 
	- 数据结构
---

### 队列

只能在一段插入另一端删除

数据对象集：一个有0个或者多个元素的有穷线性表

<!--more-->

操作集：

Queue Create(int MaxSize)

int IsFull(Queue Q,int Maxsize)

void Add(Queue Q,ElementType item)

int IsEmpty(Queue Q)

ElementType Delete(Queue Q)

#### 队列的顺序存储实现

为了使空间得到充分的使用**循环队列（当数组满了之后，又从头开始）**

- 如何实现？

由一个一维数组和一个记录队列头元素位置的变量**front**和一个记录队列尾元素位置的变量**rear**

插入元素时rear前移一位，删除元素时front前移一位

- 如何判断队列是空还是满？

用front和rear之间的距离来判断，当front和rear相差1时，队列满

为什么不是front和rear相等时未满？

相等时由两种状态，有可能为空也有可能满。

当然这个问题可以引入一个标记来解决：

1. 引入Size

每次Add时+1，Delete时-1，当Size为0是队列为空

2. 引入Tag

Add时Tag = 1，Delete时Tag = 0，当front和rear相等时判断Tag的值即可

<br>

**这里当rear与front相差1时就判断队列满**

```c
#define MaxSize 元素最大数
struct QNode{
    ElementType Data[NaxSize];
    int rear;
    int front;
};
typedef struct QNode *Queue;
```

1.Add

**难点在于如何当rear到MaxSize时再+1就又返回起点**

**这里使用取余，当rear到达最大值时取MaxSize的余，就得到0**

```c
void Add(Queue Q,ElementType item){
    if(IsFull(Q)){
        printf("队列满");
    }
    else{
        Q->rear = (Q->rear+1) % MaxSize;
        Q->Data[Q->rear] = item; //把item放到数组里
    }   
}
```

2. Delete

```c
ElementType Delete(Queue Q){
    if(IsEmpty(Q)){
        printf("队列空")；
        return;
    }
    else{
		Q->front = (Q->front + 1) % MaxSize;
        return Q->Data[Q->front];
    }
    
}
```

#### 队列的链式存储实现

使用一个**单链表**实现，插入和删除分别在两头进行，**问题在于front和rear应该分别指向链表的哪一头 **

rear需要插入数据需要放在链表的尾部，插入时只需要前一个节点的地址，front要在链表的头部的的下一个节点，方法与堆栈类似

```c
struct Node{
    ElementType Data;
    struct Node *Next;
}
struct QNode{
    struct Node *rear;
    struct Node *front;
};
typedef struct QNode *Queue;
Queue PtrQ;
```

1. Delete

```c
ElementType Delete(Queue PtrQ){
	struct Node *FrontCell;
    ElementType FrontElem;
    
    if(PtrQ->front == NULL){//若头节点之后没有节点则队列时空的
        printf("队列空");
        return;
    }
    FrontCell = PtrQ->front;//储存第二个节点
    if(PtrQ->front == PtrQ->rear) //若队列只有一个元素
        PtrQ->front = PtrQ->rear = NULL; //则删除完后队列置空
    else
        PrtQ->front = ptrQ->rear->Next;//
    FrontElem = Front->Data;
    free(FrontCell);
    return FrontElem;
}
```

2. Add

```c
void Add(Queue PtrQ,ElementType item){
    struct Node TempCell;//创建新节点
    TempCell = (Queue)malloc(sizeof(struct Node));//申请空间
    TempCell->Data = item;
    PtrQ->rear->Next = TempCell;//将链表与新节点链接起来
	PtrQ->rear = TempCell;//移动rear 
    
}
```





