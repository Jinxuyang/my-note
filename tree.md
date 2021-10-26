---
title: 数据结构-树（Tree）学习笔记
date: 2020-03-09 16:24:48
tags:
	- 数据结构
	- 学习笔记
	- 跨域
categories: 
	- 数据结构
---

[TOC]

### 树（Tree）

n个节点构成的有限集合

n = 0时，称为空树

<!--more-->

#### 非空树有以下性质：

- 树中有一个称为"根（Root）"的节点，用r表示

- 其余节点互不相交且有限，其中每个集合本身又是一棵树，称为原树的子树(SubTree)

  ![8i5Bf1.png](https://s2.ax1x.com/2020/03/10/8i5Bf1.png)

- 子树是不相交的
- 除根节点外，每个节点有且仅有一个父节点

**这些都不是树**

![8ioDsK.png](https://s2.ax1x.com/2020/03/10/8ioDsK.png)

- 一棵N个节点的树有N-1条边

#### 树的一些基本术语

1. 节点的度(Degree)：节点的子树个数
2. 树的度：树的所有节点中最大的度数（上面那个树的度为3）
3. 叶结点（Leaf）：度为零的节点，即没有子树
4. 父结点（Parent）：有子树的节点是其子树的根节点的父节点
5. 子结点（Child）
6. 兄弟结点（Sibling）：具有同一父节点的各节点彼此
7. 路径和路径长度：
8. 祖先节点（Ancestor）：沿树根到某一结点路径上的所有结点都是这个节点的祖先结点，比如从A到L，ABG都是L的祖先结点
9. 子孙结点（Descendant）
10. 结点的层次（Level）：规定根结点在1层，其他任一结点的层数是其父结点层数+1，
11. 树的深度（Depth）：树中所有结点中的最大层次就是这棵树的深度



#### 树的表示

为了节省空间和方便这里使用**兄弟-儿子表示法**

##### 兄弟-儿子表示法

| Element    |             |
| ---------- | ----------- |
| FirstChild | NextSibling |

链表的每个结点如上图

链接起来后如下图

![8k30kn.png](https://s2.ax1x.com/2020/03/11/8k30kn.png)

这样的树被称为**二叉树**

#### 二叉树（Binary Tree）

##### 二叉树的定义

二叉树是一个有穷的结点集合

这个集合可以为空

若不为空，则它是由根节点和称为其**左子树**和**右子树**的两个**不相交**的二叉树组成

##### 二叉树的五种基本形态

- 空树
- 只有根
- 只有左子树
- 只有右子树
- 左右子树都有

二叉树和其他度为2的树的不同在于二叉树的子树有左右顺序之分

##### 特殊的二叉树

- 斜二叉树（Skewed Binary Tree）

只有左子树或只有右子树

- 完美二叉树（Perfect Binary Tree）/满二叉树（Full Binary Tree）

![8kadJJ.png](https://s2.ax1x.com/2020/03/11/8kadJJ.png)



- 完全二叉树（Complete Binary Tree）

有n个结点的二叉树，对树中的结点按上图所示方式编号，编号为i的结点与满二叉树中编号为i的结点在二叉树中的位置相同

将完美二叉树的叶结点，从有往左依次删除任意个数，所形成的二叉树就是完全二叉树

下图就不是一个完全二叉树

![8kasL6.png](https://s2.ax1x.com/2020/03/11/8kasL6.png)

##### 二叉树的几个性质

- 一个二叉树第i层的最大结点数为：2^(i-1) , i >= 1

- 深度为k的二叉树有最大结点总数为：2^k - 1 ,k >= 1

  完美二叉树可以达到2^k - 1个结点

- 对于任何非空二叉树T，若n0表示叶节点的个数、n2是度为2的非叶结点个数，那么n0 = n2 + 1

##### 二叉树的抽象数据类型定义

###### 类型名称：二叉树

###### 数据对象集：

​		一个有穷结点集合。

​		若不为空，则有根节点和其左、右二叉树组成

###### 操作集：

1. Boolean IsEmpty(BinTree BT):判断二叉树是否为空
2. void Traversal(BinTree BT):遍历，按某种顺序访问每个结点
   - void PreOrderTraversal(BinTree BT):先序-根、左子树、右子树
   - void InOrderTraversal(BinTree BT):中序---左子树、根、右子树
   - void PostOrderTraversal(BinTree BT):后序---左子树、右子树、根
   - void LevelOrderTraversal(BinTree BT):层次遍历，从上到下、从左到右
3. BinTree CreatBinTree():创建二叉树

##### 二叉树的存储结构

###### 1. 顺序存储结构

**完全二叉树**可以方便的使用数组实现

共n个结点

| 结点 | A    | B    | C    | D    | E    |
| ---- | ---- | ---- | ---- | ---- | ---- |
| 序号 | 1    | 2    | 3    | 4    | 5    |

- 非根结点（i > 1）的父结点的序号是i/2
- 结点（i）的左孩子结点序号是2i（2i <= n，否则没有左孩子）
- 结点（i）的右孩子结点的序号是2i+1（2i+1 <= n,否则没有右孩子）

**一般二叉树**也可以使用数组实现，但是会造成空间浪费

###### 2. 链表存储

结点的结构：|Left|Data|Right|

```c
typedef struct TreeNode *BinTree;
typedef BinTree Position;
struct TreeNode{
    ElementType Data;
    BinTree Left;
    BinTree Right;
};
```

##### 二叉树的遍历

1. 遍历（递归）

简单但是浪费空间

1. 先序遍历

   遍历过程

   1. 访问根节点
   2. 先序遍历其左子树
   3. 先序遍历其右子树
   
   看了这个图可能能更好地理解递归的过程

![8Z98Ug.png](https://s1.ax1x.com/2020/03/12/8Z98Ug.png)

这个图更好，注意看箭头

![8Z9cG9.png](https://s1.ax1x.com/2020/03/12/8Z9cG9.png)

```c
void PreOrderTraversal(BinTree BT){
	if(BT){
        printf("%d",BT->Data);//打印这个结点的数据
        PerOrderTraversal(BT->Left);//递归地遍历左子树
        PerOrderTraversal(BT->Right);
    }
}
```

2. 中序遍历

   1. 中序遍历其左子树
   2. 访问其根节点
   3. 中序遍历其右节点

```c
void InOrderTraversal(BinTree BT){
	if(BT){
        
        InOrderTraversal(BT->Left);//递归地遍历左子树
        printf("%d",BT->Data);//打印这个结点的数据
        InOrderTraversal(BT->Right);
    }
}
```

3. 后序遍历

   1. 后序遍历其左节点
   2. 后序遍历其右节点
   3. 访问根节点

```c
void PostOrderTraversal(BinTree BT){
	if(BT){
        
        PostOrderTraversal(BT->Left);//递归地遍历左子树
        PostOrderTraversal(BT->Right);
        printf("%d",BT->Data);//打印这个结点的数据
    }
}
```

**以上三种遍历过程，经过结点的路线一样，只是访问各结点的时机不同**

2. 遍历（非递归）

   1. 中序遍历（第二次碰到结点就printf）
   
   - 遇到一个结点，就把它压栈，并去遍历它的左子树
- 当左子树遍历结束后，就从栈顶弹出这个结点并访问它
   - 然后按其右指针再去中序遍历该节点的右子树

   ```c
   void InOrderTraversal(BinTree BT){
       BinTree T = BT;
       Stack S = CreatStack(MaxSize); //创建并初始化堆栈
       while(T || !IsEmpty(S)){//循环结束只要满足结点为空且堆栈为空
           while(T){ //一直循环直至结点为空
               Push(S,T);//把结点压入堆栈
           	T = T->Left; //指针转到下一个左边的结点
           }
           if(!IsEmpty(S)){ //如果堆栈不空的话就开始Pop
               T = Pop(S); //把栈顶元素给T，并Pop
               printf("%5d",T->Data); //打印结点数据
               T = T->Right; //准到根节点的右子树
           
           }
   	}        
   }
   ```

   2. 先序遍历（第一次碰到结点就printf）

   **由于走过的路径相同，只需要改变访问结点的时机就可以在中序遍历的基础上实现先序遍历**

   ```c
   void InOrderTraversal(BinTree BT){
       BinTree T = BT;
       Stack S = CreatStack(MAxSize);
       while(T || !Empty(S)){
           while(T){
               Push(S,T);
               printf("%5d",T->Data);//与中序遍历的区别就在printf的位置
               T = T->Left;
           }
           if(!IsEmpty(S)){
               T = Pop(S);
               T = T->Right;
           }
       }
   }
   ```

   3. 后序遍历（第三次碰到结点再printf）	

   4. 层序遍历

      **二叉树遍历的核心问题：二维结构的线性化**

      问题在于当你访问完一个结点的左儿子或右儿子之后，剩下的一个儿子怎么办？如果没有存储右儿子或者自己，那么这些结点就丢失了，所以需要一种方法保存该节点或保存他的父结点

      

      **总的来说就是我们需要一个存储结构保存暂时不访问的结点**

      这里使用队列解决问题

      - 先把根入队
      - 根出队，并且让它的两个儿子入队，左儿子现右儿子后
      - 依次让队列里的结点出队，并且让他的儿子入队
      - 重复，直至队列空

      

      ```c
      void LevelOrderTraversal(BinTree BT){
          Queue Q;
          BinTree T;
          Q = CreatQueue(MaxSize); //创建并初始化队列
          Add(Q,BT); //让根入队
          while(!IsEmpty(Q)){ //队列不空就一直循环
              T = Delete(Q); //队首的出队，并记录队首的地址
              printf("%d\n",T->Data);
              if(T->Left) Add(Q,T->Left); //左儿子入队
              if(T->Right) Add(Q,T->Right); //右儿子入队
          }
      }
      ```

###### 二叉树遍历的应用

##### 二叉树的同构

一个二叉树可以通过n次左右树交换就能变得和另一个一样，就称俩数同构 

#### 二叉搜索树（Binary Search Tree）

满足：

1. 非空左子树的所有键值小于其根节点的键值
2. 右子树大于根节点
3. 左右子树都是搜索二叉树

![GR6Tpt.png](https://s1.ax1x.com/2020/04/08/GR6Tpt.png)

##### 操作集

Position Find(ElementType X,BinTree BST):查找X，返回结点的地址

Position FindMin(BinTree BST)：返回最小元素结点并返回

Position FindMax(BinTree BST)：最大

BinTree Insert(ElementType X,BinTree BST)：插入X

BinTree Delete(ElementType X,BinTree BST)：删除X



Find()思路：

1. 从根节点开始，如果树为空返回NULL
2. 非空就和X进行比较
   1. 若小于根就在左子树继续查找
   2. 大于则就在右子树里查找
   3. 相等就返回指针



递归实现

```c
Position Find (ElementType X,BinTree BST){
	if(!BST) return NULL;//若数为空就返回NULL
    if(x > BST->Data)
        return Find(X,BST->Right); //递归地调用Find，进入右子树继续查找
    else if(X < BST->Data)
        return Find(X,BST->Left);
    else
        return BST;//相等时返回该节点的指针
}
```

循环实现

```c
Position IterFind(ElementType X,BinTree BST){
    while(BST){
        if(X > BST->Data)
            BST = BST->Right;
        else if(X < BST->Data)
            BST = BST->Left;
        else
            return BST;
    }
    return NULL;
}
```



FindMin()&FindMax()

递归实现

```c
Position FindMin(BinTree BST){
    if(!BST) return NULL;//空就返回NULL
    else if(!BST->Left)//如果左子树为空，说明到最小回
        return BST;
    else
        return FindMin(BST->Left);//不为空，就进入左子树
}
```

循环实现

```c
Position FindMax(BinTree BST){
	if(BST)//找到空为止
        while(BST->Right) BST = BST->Right;
    return BST;
}
```

Insert()

**关键是找到插入的位置**

```c
BinTree Insert(ElementType X,BinTree BST){
    if(!BST){//若原树为空，则生成并返回一个结点
        BST = malloc(sizeof(struct TreeNode));
        BST->Data = x;
        BST->Left = BST->Right = NULL;
    }
    else{
        if(x < BST->Data)
            BST->Left = Insert(X,BST->Left);//递归，找出正确位置，赋值
        else if(X > BST->Data)
            BST->Right = Insert(X,BST->Right);
        //else 若X已经存在，那什么都不用做
        return BST;
    }
}
```

Delete()

有三种情况

1. 叶结点，直接让他的父结点指向NULL
2. 只有一个孩子，直接用它的儿子替代它‘
3. 有两个孩子，

```c
BinTree Delete(ElementType X,BinTree BST){
    Position Tmp;
    if(!BST) printf("要删除的元素未找到");
    else if(X < BST->Data)
        BST->Left = Delete(X,BST->Left);//递归
    else if(X > BST->Data)
        BST->Right = Delete(X,BST->Right);//这两个else if都是查找的过程
    else//找到后
        if(BST->Left && BST->Right){//判断结点的类型
            Tmp = FindMin(BST->Right);//找到右子树中最小的结点
            BST->Data = Tmp->Data;//用找到的那个结点替换要删除的结点
            BST->Right=Delete(BST->Data,BST->Right);//删除那个用于替换原结点的结点,不理解为什么，有赋值这个操作 
        }
    	else{
            Tmp = BST;
            if(!BST->Left)
                BST = BST->Right;
            else if(!BST->Right)
                BST = BST->Left;
            free(Tmp);
        }
    return BST;
}
```

#### 平衡二叉树

查找效率高

**平衡因子（Blance Factor，BF）**：BF(T) = hL-hR

hL,hR分别为T的左右子树的高度



**平衡二叉树(Blance Binary Tree)(AVL树)**

1. 空树
2. 任意结点左右子树高度差的绝对值不超过1

##### 平衡二叉树的调整

给树插入结点时，有可能会破坏树的平衡，因此需要调整

平衡二叉树还是一个搜索二叉树，因此在调整过程中，依然要保持搜索二叉树的特性

#### 堆

优先队列(Priority Queue):特殊的队列，取出元素的顺序按照元素的优先权（关键字）大小

**两个特性：**

1. 结构性：用数组表示的完全二叉树
2. 有序性：任意结点的关键字是其子树所有结点的最大值或最小值
   - 最大堆（MaxHeap）也称大顶堆
   - 最小堆（MinHeap）也称小顶堆



##### 操作集

MaxHeap Create(int MaxSize)

Boolean IsFull(MaxHeap H)

Inser(MaxHeap H,ElementType item)

Boolean IsEmpty(MaxHeap H)

ElementType DeleteMax(MaxHeap H)

```c
typedef struct HeapStruct *MaxHeap;
struct HeapStruct{
    ElementType *ElementType;//储存堆元素的数组
    int Size;//堆当前元素的个数
    int Capacity;//堆的最大容量
}
```

```c
MaxHeap Create(int MaxSize){
    MaxHeap H = malloc(sizeof(struct HeapStruct));//申请一块空间给H
    H->Elements = malloc((MaxSize+1)*sizeof(ElementType));//申请一块数组空间
    H->Size = 0;//初始为零，当前元素个数
    H->Capacity = MaxSize;//初始为MaxSize，堆的最大容量
    H->Element[0] = MaxDate;//哨兵，方便以后访问
    return H; 
}
```

```c
void Insert(MaxHeap H,ElementType item){
    int i;
    if(IsFull(H)){
        printf("满");
        return;
    }
    i = ++H->Size;//i指向插入后堆中的最后一个元素,插入后Size++
    for(;H>Elements[i/2] < item;i/=2)//插入时结点放在最后，然后依次与他的父结点进行比较，直到父结点大于他为止
        H->Elements[i] = H->Elements[i/2];//大于就和父结点互换位置
    H->Elements[i] = item;
    //完全二叉树中，一个结点的序号/2就是它父结点的序号
}
```



[![G4l1bV.png](https://s1.ax1x.com/2020/04/09/G4l1bV.png)](https://imgchr.com/i/G4l1bV)



哨兵的一个作用就是，哨兵的值是堆中最大的，无论多大的结点来比较，到哨兵这里就会停止，可以减少一个判断条件i>1，提高效率

**基本思路：删除根之后，把树的最后一个节点（保证完全二叉树的特性）挪到根的位置，然后进行调整排序**

```c
ElementType DeleteMax(MaxHeap H){
    int Parent,Child;
    ElementType MaxItem,temp;
    if(IsEmpty(H)){
        printf("满");
        return;
    }
    MaxItem = H->Elements[1];//把要删除的结点存起来，一会儿返回出去
    temp = H->Element[H->Size--];//把最后一个结点存起来，然后Size--
    for(Parent = 1;Parent*2<=H->Size;Parent=Child){//从根节点开始循环，每次循环完后进入下一层的左节点，如果Parent*2<=H->Size说明，下一层没有节点了
        Child = Parent*2;//进入下一层的左节点
        if((Child!=H->Size)&&(H->Element[Child]<H->Elements[Chile+1]))//Child!=H->Size保证进入这个判断的结点都有两个儿子，(H->Element[Child]<H->Elements[Chile+1])默认左节点大于右节点，如果小于就Child++
            Child++;
        if(temp >= H->Element[Child]) break;//如果temp大于这个节点的最大子节点那么说明位置正确
        else H->ElementS[Parent] = H->Elements[Child];//交换两节点位置
    } 
    H->Element[Parent] = temp;
    return MaxItem;
}
```

**建立最大堆**

方法：

1. 通过Insert操作，将元素一个一个插进去（O(NlogN)）

2. 在线性时间复杂度下建立最大堆

   1. 将N个元素按顺序存入，先满足完全二叉树的特性
   2. 调整各节点位置，以满足有序特性

   **如何调整**

#### 哈夫曼树（最优二叉树）（Huffman Tree）

**带权路径长度（WPL）：**设二叉树有n个叶结点，每个叶结点带有权值Wk，从根节点到每个叶结点的长度为Lk，WPL=每条路径长度乘权值的和

哈夫曼树：WPL最小

##### 哈夫曼树的构造

思路:把所有元素按权值排列，然后拿出两个权值最小的合并成一个二叉树，然后再找两个最小的合并，直到合并完

```c
typedef struct TreeNode *HuffmanTree;
struct TreeNode{
    int Weight;
    HuffmanTree Left,Right;
}
HuffmanTree Huffman(MinHeap H){//假设H->Size的权值已经存在H->Element[]->Weight里
    int i;
    HuffmanTree T;
    BulidMinHeap(H); //将H-》ElementS[]按权值调整为最小堆
    for(i = 1;i<H->Size;i++){
        T = malloc(sizeof(struct TreeNode));//建一个新结点，存放新组成的树
        T->Left = DeleteMin(H);
        T->Right = DeleteMin(H);//从堆里拿出俩元素，进行组合
        T->Weight = T->Left->Weight+T->Right->Weight;
        Insert(H,T);//把组合后的树插入最小堆
    }
    T = DeleteMin(H);
    return T;
}
```

**特点**：

- 没有度为1的结点
- n个叶结点的哈夫曼树共有2n-1个结点
- 任意非叶结点的左右子树交换后仍是哈夫曼树
- 同一组权值存在不同构的两颗哈夫曼树

##### 哈夫曼编码

为了将字符的存储空间降到最小还要避免二义性，就可以使用哈夫曼编码（编码不等长）

方法：

1. 将每个字符出现的次数作为权值
2. 创建一个哈夫曼树，保证每个字符都在叶结点上就不会出现二义性

#### 集合

并查集：集合并，查某元素属于那个集合

存储实现：用树结构（并非二叉树），树的每个结点代表一个元素

1. 用是链表实现

2. 数组：

   - 数组的每个分量都是一共结构，包含结点的值和父结点的下标，没有父结点则记为-1

   ```c
   typedef struct{
       ElementType Data;
       int Parent;
   }SetType;
   ```

   - 查

   ```c
   int Find(SetType s[],ElementType X){
       
       int i;
       for(i = 0;i < MaxSize && S[i].Data != X;i++);//循环查找X，找到后退出时i的值就是该节点的下标或没找到
       if(i >= MaxSize) return -1;//判断属于以上那种情况
       for(;s[i].Parent >= 0;i = s[i].Parent);//查找这个结点的根结点，退出时就是i是根节点的下标
       return i ;
   }
   ```

   - 并
     - 分别找到两个集合的根节点
     - 若不同根就设置一个集合的根节点的父结点为另一个集合的根节点

   ```c
   void Union(SetType s[],ElementType X1,ElementType X2){
       int Root1,Root2;
       Root1 = Find(S,X1);
       Root2 = FInd(S,X2);
       if(Root1 != Root2) s[Root2].Parent = Root1;
   }
   ```

   ​	如果总是这样插的话可能会导致树越来越高，所以考虑将结点少的插到结点多的树底下

   这使就需要考虑如何存储一个树的结点个数，如果在结构中再创建一个变量的话，由于只有根节点需要存储数据，会造成空间浪费，所以考虑继续使用本来的数组存储，可以使用原来标记结点为根节点的空间来存储，有x个结点就在数组中存为-x，到时候只需要判断正负就可以。

