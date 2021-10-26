---
title: 线性表
date: 2020-03-03 16:28:46
tags:
---

### 什么是线性表

线性表（Linear List）由**同类型**数据元素构成**有序序列**的线性结构

- 表中元素个数称为线性表的**长度**

- 线性表没有元素时，称为**空表**

- 表起始位置称为**表头**，结束位置称为**表尾**

  <!--more-->

数据对象集：n个元素构成的有序序列  

### 操作集

**L表示一个线性表，整数i表示位置，元素X属于ElementType**

**ElementType表示一种数据类型，可以是整形也可以是实型，也可以是结构**

<br>

List MakeEmpty():初始化一个空线性表

ElementType Findkth(int k,LIst L):返回位序K的元素

int Find(ElementType X,List L):查找X在L内第一次出现的位置

void insert(ElementType X,int i,List L):给L在位序i前插入一个元素X

void Delete(int i,List L):删除L内位序为i的元素

int Lenth(List L):返回L的长度

#### 线性表的顺序存储实现

```c++
typedef struct LNode *List//List存放该结构的地址
struct LNode{//定义一个名为LNode的结构
    ElementType Data[MAXSIZE];
    int Last;//线性表最后一位的位序
}；
struct LNode L; //声明一个结构体
List PtrL; //声明该结构体的指针
```

访问 下标为i的元素：L.Data[i]或PtrL->Data[i]

线性表的长度：L.Last+1或PtrL->Last+1    因为Last从0开始，所以长度为Last+1

**"->"表示取出PtrL指向的结构体中的某个数据，与"."类似，当声明一个指针变量时想要取出该结构体中的数据就需要"->",而声明一个普通的变量是使用"."即可**

#### 主要操作实现

1. 初始化（建立空顺序表）

   ```c++
   List MakeEmpty(){
       List Ptrl; //声明该结构体的指针
       PtrL = (List)malloc(sizeof (struct LNode));
       Ptrl->Last = -1;//当表内没有数据时Last为-1
       return Ptrl;
   }
   ```


2. 查找  O(n)

```c
int Find(ElementType X,List PtrL){
    int i = 9;
    while(i <= PtrL->Last && PtrL->Data[i]!= x){//循环结束有两个原因一个是i>last说明，找完了还没有，另一个是Data[i] = X说明找到了
        i++;
    }
    if(i > PtrL->Last)
        return -1;
    else 
        return i;
}
```

3. 插入（第i（1<=i<=n+1）个位置上插入一个值位X的新元素）

在第i个位置插入实际上就是插在下标位i-1的位置，首先把原来的数据从i-1开始依次向后移（从后往前），然后把数据插到i-1，

```c
void Insert(ElementType X,int i,List PtrL){
    int j;
    if(PtrL->Last == MAXSIZE-1){ //判断表的最后一位是否已经到达MAXSIZE，-1是因为表的下标从0开始
        printf("表满");
        return;
    }
    if(i < 1 || i > PtrL->Last+2){ //或者可以写成（i-1 < 0 || i-1 > PtrL->Last+1）+1确保还有剩余位置
        printf("位置不合法");
        return;
    }
    for(j = PtrL->Last;j >= i-1;j++){ //从最后一位开始，循环到i-1这个位置（O(n)）
        Ptrl->Data[j+1] = Ptrl->Data[j];//每个数据往后移动一位
    }
    PtrL->Data[i-1] = X;//令原本下标为i-1的位置为X
    PtrL->Last++;//表长+1
}
```

4. 删除（第i个位置）

```c
void Delete(int i,List PtrL){
    int j;
    if(i < 1 || PtrL->Last+1){ //此处为删除因此不需要保存余量，只需小于Last+1即可
        printf("不存在");
        return;
    }
    for(j = i;j <= PtrL->Last;j++){//从i+1开始到结束的值都向前移动一位
        PtrL->Data[j-1] = PtrL->Data[j];
    }
    PtrL->Last--;
    return;
}
```

#### 线性表链式存储实现

在链表内插入只需要修改链，但是查找第i个元素和查看链表长度就比较复杂

```c
typedef struct LNode *List;
struct LNode{
    ElementType Data;//存储数据
    List Next;//下一个链表的头。
};
Struct LNode L;
List PrtL;
```

1. 求表长

```c
int Length(List PtrL){
    List p = PtrL; //p指向表的第一个节点
    int j = 0;
    while(p){//若返回NULL即到最后一位，则循停止
        p = p->Next; //指向下一个节点
        j++;//计数
    }
    return j;
}
```

2. 查找

   1. 按序号查找

      ```c
      List FindKth(int K,List PtrL){
          List p = PtrL;
          int i = 1; //表头为一
          while(p != NULL && i < K){//第一个条件是表不能到结尾，第二个是刚好遍历到K就停止
              p = p->Next;//转到下一节点
              i++;//计数
          }
          if(i == K) //等于说明找到了
              return p;//返回该节点的指针
          else
              return NULL;
      }
      ```

   2. 按值查找

      ```c
      List Find(ElementType X,List PtrL){
          List p = Prtl;
          while(X != P->Data && p != NULL){
              p = p->Next
          }
          if(p->Data == X)
      		return p;
          else
              return NULL;
      }
      ```
   
3. 插入（在i-1（1<=i<=n+1）个节点后插入一个值为X的新节点）**之所以插入到i-1之后是因为，想给链表插入节点，需要知道前面一个节点的信息**

```c
List insert(ElementType X,int i,List PtrL){//List PtrL 传入的是表头的指针
    List p,s;
    if(i == 1){//1在表头位置需要特殊处理
        s = (List)malloc(sizeof(struct LNode));//申请一块空间
        s->Data = X;
        s->Next = PtrL;//将这下个节点的Next赋给此节点的Next
        return s;//返回表头
    }
    p = FindKth(i-1,PtrL);
    if(p == NULL){
        printf("error");
        return NULL;
    }
    else{
        s = (List)malloc(sizeof(struct LNode));
        s->Data = X;
        s->Next = p->Next;//把i-1的链接给要插入的节点
        p->Next = s;//把要插入的节点的链接给i-1
        return PtrL;
    }
}
```

4. 删除

```c
List Delete(int i,List PtrL){
	List p,s;
    if(i == 1){//1特殊化
        s = PtrL;
        if(PtrL!=NULL) //查看这个表是否无节点
            PtrL = PtrL->Next; //是表头变为下一个节点
        else
            return NULL;
        free(s); //释放被删除的节点
        return PtrL;
    }
    p = FindKth(i-1,List PtrL); //查找第i-1个节点
    if(p == NULL){ //判断输入的i是否在范围内
        printf("第%d个节点不存在"，i-1);
    }
    else if(p->Next == NULL){  //判断输入的节点是否是表的结尾
        printf("第%d个节点不存在"，i);
    }
    else{
        s = p->Next;
        p->Next = s->Next;
        free(s);
        return PtrL;
    }
}
```





