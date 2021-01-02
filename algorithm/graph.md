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



## 最短路径算法（Shortest Path Algorithm）

**Dijkstra**

```java
public void dijkstra(int s, int t) {                // 从顶点s到顶点t的最短路径
  int[] predecessor = new int[this.v];              // 用来还原最短路径
  Vertex[] vertexes = new Vertex[this.v];           // 初始化所有顶点的 dist 都初始化为无穷大
  for (int i = 0; i < this.v; ++i) {                
    vertexes[i] = new Vertex(i, Integer.MAX_VALUE);
  }
  PriorityQueue queue = new PriorityQueue(this.v);  // 小顶堆
  boolean[] inqueue = new boolean[this.v];          // 标记是否进入过队列
  vertexes[s].dist = 0;                             // 起始顶点的 dist 值初始化为 0
  queue.add(vertexes[s]);                           // 将起始顶点其放到优先级队列中
  inqueue[s] = true;                                // 起始顶点标记入过队列
  while (!queue.isEmpty()) {
    Vertex minVertex= queue.poll();                 // 取堆顶元素并删除
    if (minVertex.id == t) break;                   // 最短路径产生了 s->...->t
    for (int i = 0; i < adj[minVertex.id].size(); ++i) {  // 遍历当前最近顶点的邻接边
      Edge e = adj[minVertex.id].get(i);            // 取出一条minVetex相连的边
      Vertex nextVertex = vertexes[e.tid];          // 获取当前邻接边下一个顶点nextVertex
      if (minVertex.dist + e.w < nextVertex.dist) { // 如果当前路径更短             ***
        nextVertex.dist = minVertex.dist + e.w;     // 更新顶点nextVertex的dist    ***
        predecessor[nextVertex.id] = minVertex.id;  // 更新最短路径队列predecessor  ***
        if (inqueue[nextVertex.id] == true) {       // 更新队列中的dist值
          queue.update(nextVertex);                 
        } else {
          queue.add(nextVertex);                  
          inqueue[nextVertex.id] = true;
        }
      }
    }
  }
  System.out.print(s);                             // 输出最短路径
  print(s, t, predecessor);                        // s->...->t
}
```

