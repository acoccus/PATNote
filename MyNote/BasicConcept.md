### 图的概念

1. 图至少有一个顶点，可以没有边

2. Undirected Graph   无向边(Edge)   Edge(u, v) = Edge（v, u) 

   Directed Graph 	有向边(arc) 	arc<u, v> != arc(u, v)

3. non - Simple Graph ： 如果存在重边或者自回路边，就叫做非简单图。一般只考虑简单图。
4.  Simple Graph :  (strict graph) is  an  graph containing no graph loops or multiple edges
5. cycle : 有向图中的一条回路是指**起点 == 终点**的一条路径。

6. simple cycle

   is a circuit in which the **only** repeated vertices are the first and last vertices.

7. simple path

   路径中只有起点和终点可以重复。

   1）起点==终点：所有顶点除了终点外，任意两点均不重复。  == >  simple cycle

   2）起点！=终点: 任意两点均不重复

8. 自回路(Self-loop)：路径长度为1的一个回路

9. Undirected Complete Graph:

   a simple undirected graph in which every pair of distinct vertices is connected by a unique edge.

   n个顶点的无向完全图，有n*(n-1) / 2条边。

10. Directed Complete Graph:

    is a directed graph in which every pair of distinct vertices is connected by a pair of unique edges (one in each direction).

    n个顶点的有向完全图，有n*(n-1)条边

11. Dense Graph 稠密图         Sparse Graph 稀疏图

    - density = 2|E| / |V|

      稠密图 |E| = k |V ^ 2| ( 1/8 < k < 1/2)

12. Connected Graph

    在无向图中，如果图中任意两顶点都是连通的，则称该图是连通图。 无向图的极大连通子图称为连通分量。

    Connected Component：

    1. 子图
    2. 连通
    3. 极大顶点数
    4. 极大边数

13. Strongly Connected Graph 

    有向图中，从vi 可以到 vj ， 从vj也可以到vi， 则称vi ， vj 强连通。如果图中任意两顶点都是强连通的，则称该图是强连通图。 有向图的极大强连通子图称为强连通分量。

14. Spanning Tree

    G 包含其全部n个顶点的一个极小连通子图。它一定有n-1条边。

15. 树和图的关系

    下列任意条件为的图G退化为树的充要条件。

    G有n-1条边，且没有环

    G有n-1条边，且是连通的

    G中的每一对顶点有且只有一条路径相连

    G是连通的，但删除任何一条边就会使它不连通。