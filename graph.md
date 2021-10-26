---
title: 图（Graph）
date: 2020-04-26 09:40:33
tags:
---

[TOC]

### 图（Graph）

- 表示多对多的关系
- 包含
  - 一组顶点:通常用V（Vertex）表示顶点集合
  - 一组边：通常用E（Edge）表示边的集合
    - 边是顶点对：（v,w）属于E，v,w属于V
    
    - 有向边<v,w>表示从v指向w的边（单行线）
    
    - 不考虑重边和自回路
    
      <!--more-->

#### 抽象数据类型定义

- 类型名称：图（Graph）
- 操作对象集：G（V，E）由一个非空的有限顶点集合V和一个有限边集合E组成
- 操作集
  - Graph Create()
  - Graph InsertVertex(Graph G,Vertex V)
  - Graph InsertEdge(Graph G,Edge e)
  - void DFS(Graph G,Vertex v)
  - void BFS(Graph G,Vertex v)
  - void ShortestPath(Graph G,Vertex v,int Dist[])
  - void MST(Graph G)最小生成树

#### 如何表示一个图

- 邻接矩阵
  - 有边记为1
  - 优缺点
    - 直观简单，方便查找任意一对顶点之间是否有边，方便查找任一顶点的所有邻接点（有边直接相连的顶点）
    - 方便计算任意顶点的度
      - 无向图：对应行或列非零元素个数
      - 有向图：非零行是出度，非零列的入度
    - 若是稀疏矩阵的话会造成空间浪费以及时间浪费

```c++
typedef struct GNode *PtrToGNode;
struct GNode{
    int Nv;//定点数
    int Ne;//边数
    WeightType G[MaxVertexNum][MaxVertexNum];
    DataType Data[MaxVertexNum]; //顶点包含的数据
}
typedef PtrToGNode MGraph;
```

MGraph创建并初始化

```c
typedef int Vertex;
MGraph CreateGraph(int VertexNum){
    Vertex V,W;
    MGraph Graph;
    Graph = (MGraph)mallcon(sizeof(struct GNode));
    Graph->Nv = VertexNum;
    Graph->Ne = 0;
    
    //把Graph全部置为0
    for(V = 0;V < Graph->Nv;V++){
		for(W = 0;W < Graph->Nv;W++){
            Graph->G[V][W] = 0;
        }
    }
}
```

向MGraph插入边

 ```c
typedef struct ENode *PtrToENode;
struct ENode{
	Vertex V1,V2;
    WeightType Weight;
};
typedef PtrToENode Edge;
void InsertEdge(MGraph Graph,Edge E){
	Graph->G[E->V1][E->V2] = E->Weight;
    //若为无向图，则还要插入边（V2，V1）
    Graph->G[E->V2][E->V1] = E->Weight;
}
 ```

建立完整的MGraph

```c
MGraph BuildGraph(){
	MGraph Grpah;
    Edge E;
    Vertex V;
    int Nv,i;
    
    scanf("%d",&Nv);
    Graph = CreateGraph(Nv);
    scnaf("%d",&(Graph->Ne));
    if(Graph->Ne != 0){
		E = (Edge)malloc(sizeof(struct ENode));
        for(i = 0;i < Graph->Ne;i++){
			scanf("%d %d %d",&E->V1,&E->V2,&E->Weight);
            InsertEdge(Graph,E);
        }
        
        //读入数据
        for(V = 0;V < Graph->Nv;V++){
            scanf("%c",&(Graph->Data[V]));
        }
        return Graph;
    }
}
```

**这个矩阵存的是顶点与顶点间的关系**

- 邻接表
  - 用一个指针数组，对应矩阵每行一个元素，只存非零元素
    - 优缺点
      - 对于稀疏矩阵在时间和空间上的表现都比较好
      - 方便找任意顶点的邻接点
      - 方便计算任一顶点的度
        - 无向图：是
        - 有向图：否，只能计算出度，需要构造逆邻接表
      - 难以检查任意一对顶点间是否存在边
- 用一个长为N(N+1)/2的一维数组表示
  - Gij在这个数组中对应的下标为(i*(i+1)/2+j) 

```c
typedef struct Vnode(){
	PtrToAdjVNode FirstEdge;
    DataType Data;
}AdjList[MaxVertexNum];

typedef struct GNode *PtrToGNode;
struct GNode{
	int Nv;
    int Ne;
    AdjList G;
};
typedef PtrToGNode LGraph;

```



#### 图的遍历

##### 深度优先搜索(Depth First Search,DFS)

思路:从起点出发，挑视线内一盏灯点亮，然后走到刚刚点亮的等上，继续重复，如果视线内所有灯都被点亮，就原路返回，退后一格，然后继续看，直到退到起点

```c
void DFS(Vertex V){
	visited[V] = ture;
    for(V的每个邻接点W){
		if(!visited[W]) DFS(W);
    }
}
```

##### 广度优先搜素(Breath First Search,BFS)

与树的层序遍历类似

思路：从起点开始，把所有邻接点压入队列，然后弹出一个，再把这的结点的所有邻接点压入队列，重复直至队列空

```c
void BFS(Vertex V){
    visited[V] = true;
    Enqueue(V,Q);
    while(!IsEmpty(Q)){
        V = Dequeue(Q);
        for(V的每个邻接点W){
			if(!visited[W]){
				visited[W] = true;
                Enqueue(W,Q);
            }
        }
    }
}
```

#### 最短路问题

分类：

- 单源最短路径问题：从某固定点出发，求其到所有其他顶点的最短路径
  - 有向无权图
  - 有向有权图
- 多源最短路问题：求任意两顶点间的最短路径

##### 单源有向无权图的最短路

**无权图的最短路可以认为是，从起点到终点经过的顶点数最少的路**

与BFS有点类似

```c
void Unweighted(Vertex S){
	Enqueue(S,Q);//先把这个结点入队
    while(!IsEmpty(Q)){
		 V = Dequeue(Q);//弹出一个元素
    	for(V 的每个邻接点 W)//遍历V的每个邻接点
        if(dist[W] == -1){//如果W没有被访问过就执行以下操作
			dist[W] = dist[V]+1;//S到W的距离变成，S到他前一个结点V的距离+1
            path[W] = V;//要到达W就要经过V，把v存起来
            Enqueue(W,Q);//把这个结点入队
        }
    }
}
```

因为每次找的都是这个结点的邻接点，因此最后出来的结果是最短的

path存储来这个结点的结点，只要一直网下推，就能推出整条最短路

##### 单源有向有权图

**权重和最小 **

**Dijkatra算法**

- 令s = {源点s + 已经确定了从s到的最短路径的顶点v}
- 对于任何一个没有收录的顶点v，定义dist[v]为s到v的最短路径长度，但该路径**仅经过s中的顶点**，即{s -> (v属于s) -> v}的最小长度(这个最小长度不是最终的最小长度)
  - 前提条件：路径是按照递增的顺序生成的
  - 真正的最短路必须只经过s中的顶点，因为，假设一个顶点不在s中，但从源点到v经过他，这时就产生了矛盾，因为路径书按照递增顺序生成的
  - 每次从未收录的顶点中选一个dist最小的收录
  - 增加一个v进入s，可能会影响另外一个w的dist值 
    - 要产生这样的结果要满足
    - v在s到w的路径上
    - v到w一定有一条直接的边，因为路径按照递增顺序生成

```c
void Dijkstra(Vertex s){
    while(1){
        V = 为收录顶点中dist最小者;
        if(这样的V不存在) break;
        collected[V] = true;//用来标记结点是否被收录
        for(V 的每个邻接点 W)
            if(collected[W] == false)
                if(dist[V] + E(v到w的距离) < dist[W]){//如果原来s到w的距离大于，s到v加上v到w的距离那么就改变s到w的最短路径
                    dist[W] = dist[V]+E(V到W的距离);
                    path[W] = V;
                }
    }
}
```

