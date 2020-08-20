Graph
---------

## Basic

**无向图**

![img](images/df85dc345a9726cab0338e68982fd1af.jpg)

**有向图**

![img](images/c31759a37d8a8719841f347bd479b796.jpg)

- 顶点（vertex）
- 边（edge）
- 度（degree）: 跟顶点相连接的边的条数。
- 入度（In-degree，有向图）：表示有多少条边指向这个顶点
- 出度（Out-degree，无向图）：表示有多少条边是以这个顶点为起点指向其他顶点

**带权图 weighted graph**

![img](images/55d7e4806dc47950ae098d959b03ace8.jpg)

## 存储方法

**邻接矩阵（Adjacency Matrix）**

![img](images/625e7493b5470e774b5aa91fb4fdb9d2.jpg)

对于无向图来说，如果顶点 i 与顶点 j 之间有边，我们就将 $A[i][j]$ 和 $A[j][i]$ 标记为 1；

对于有向图来说，如果顶点 i 到顶点 j 之间，有一条箭头从顶点 i 指向顶点 j 的边，那我们就将 $A[i][j]$ 标记为 1。

优点：存储简单，读取高效

缺点：占用空间大，稀疏图浪费空间

**邻接表（Adjacency List）**

![img](images/501440bcffdcf4e6f9a5ca1117e990a1.jpg)

邻接表中，每个顶点的链表中，存储的就是这个顶点指向的顶点。

逆邻接表中，每个顶点的链表中，存储的是指向这个顶点的顶点。

## 搜索

### 广度优先搜索 Breadth-First-Search

![img](images/002e9e54fb0d4dbf5462226d946fa1ea.jpg)

### 深度优先搜索Depth-First-Search

![img](images/8778201ce6ff7037c0b3f26b83efba85.jpg)