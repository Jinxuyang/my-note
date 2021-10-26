---
title: 数据结构-堆栈（Stack）学习笔记
date: 2020-03-06 10:29:13
tags:
	- 数据结构
	- 学习笔记
categories: 
	- 数据结构
---

### 堆栈

数据对象集：一个有0个或多个元素的有穷线性表

操作集：

1. Stack CreateStack(int MaxSize)
2. bool IsFull(Stacak S,int MaxSize)
3. void Push(Stack S,ElementType item)
4. bool IsEmpty(Stack S)
5. ElementType Pop(Stack S)

<br>

<!--more-->

#### 堆栈的顺序存储实现

通常由一个**一维数组**和一个**记录栈顶元素位置的变量**组组成

```c
#define MaxSize 元素最大个数
typedef struct SNode *Stack;
struct SNode{
    ElementType *Data;
    int Top;
    int MaxSize;
};
```

1. 创建

```c
Stack CreateStack(int MaxSize){
	Stack S = (Stack)malloc(sizeof(struct SNode));//申请一块空间，存放Stack这个结构
    S->Data = (ElementType *)malloc(MaxSize * sizeof(ElementType))//申请一块空间存放MaxSize个ElementType
    S->Top = -1;//初始状态栈顶为-1
    S->MaxSize = MaxSize;
    return S;
}
```

2. 栈是否满了

```c
bool IsFull(Stack S,int MaxSize){
    if(S->Top == S->MaxSize - 1)
        return true;
    else
        return false;
}
```

3.栈是否为空

```c
bool IsEmpty(Stack S){
    if(S->Top == -1)
        return true;
    else
        return false;
}
```

4. Push

```c
bool Push(Stack S,ElemenType item){
    if(IsFull(S) == true)
        printf("栈满")；
        return false;
    else{
        S->Data[++(S->Top)] = item; //Top先加一，在进行运算
        return true;
    }
}


```

**++a是先自加再进行运算，a++是先运算再自加**

5. Pop

```c
ElementType Pop( Stack S ){
	if ( IsEmpty(S) ){
        printf("栈空");
        return false;
    }
    else{
        
        return s->Data[(S->Top)--];
    }
}
```

<br>

#### 用一个数组实现两个堆栈

思路：两个堆栈分别从数组头和尾开始，向中间，当两个顶指针相遇时，表示两个堆栈都满了

​			----->    <-------

```c
#define MaxSize 元素最大个数
typedef struct DStack *PtrS;
struct DStack{
    ElementType *Data;
    int Top1;
    int Top2;
};

```

​    S.Top1 = -1    说明栈1空

​    S.Top2 = MaxSize    说明栈2空

1. Push

```c
void Push(PtrS S,ElementType item,int Tag){
    if(S->Top2 - S->Top1 == 1){//两栈相遇时Top相差1
        printf("堆栈满");
        return;
    }
    if(Tag == 1)
        S->Data[++(S->Top1)] = item;
    else
        S->Data[--(S->Top2)] = item;//注意因为堆栈2是倒着来的，所以是--
}
```

2. Pop

```c
ElementType Pop(PtrS S,int Tag){
	if(Tag == 1){
        if(S->Top1 == -1){
            printf("堆栈1空")；
            return;
        }
        else{
            return S->Data[(S->Top1)--];
        }
	}
    else{
        if(S->Top2 == MaxSize){
            printf("堆栈1空")；
            return;
        }
        else{
            return S->Data[(S->Top2)++];
        }
    }
}
```

#### 堆栈的链式存储实现

实际上是单向链表，叫链栈。插入和删除操作只能再链栈的栈顶进行，**栈顶指针应该在链表的表头后其他节点之前**，否则无法进行删除操作，因为前一个节点无法保存上一个节点的指针

```c
typedef struct SNode *Stack;
struct SNode{
    ElementType Data;
    struct SNode *Next; //记录下一个节点的指针
};
```

1. Create

```c
Stack CreateStack(){//构建指针头节点
    Stack S;
    S = (Stack)malloc(sizeof(struct SNode));
    S->Next == NULL;
    return S;
}
```

2.IsEmpty

```c
int IsEmpty(Stack S){
    return (S->Next == NULL); //若头节点的Next为NULL则堆栈为空
}
```

3. Push

   ```c
   void Push(ElementType item,Stack S){
       Stack TempCell;
       TempCell = (Stack)malloc(sizeof(Struct SNode));
       TempCell->Data = item;
       TempCell->Next = S->Next;
       S->Next = TempCell; //插到头节点之后其他节点之前
   }
   
   ```


4. Pop 

```c
ElementType Pop(Stack S){
    if(IsEmpty(S)){
        printf("堆栈空");
        return NULL;
    }
    else{
        Stack TopCell;//为了找到第三个节点和释放空间而声明的
        ElemtType TopElem;
    	TopCell = S->Next;//把第二个节点的地址给TopCell
    	S->Next = TopCell->Next;//把第三个节点给S的Next，就跳过了第二个节点即删除了第二个节点
		TopElem = TopCell->Element;        
        free(TopCell);
        return TopElem;
    }
    
}
```

#### 堆栈的应用

- 中缀表达式转后缀表达式
- 函数调用及递归实现
- 深度优先搜索
- 回溯算法
- .......

