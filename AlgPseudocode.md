## 考纲回顾

### 重要算法

### DFS

``` C++
int vis[MAXN]; // Maximum Number of Vertex
void DFS(Graph G, Vertex U)
{
    Do(U); // do sth
    vis[U] = true;
    
    
    
    for(U 的每个邻接点 V)
        if(! vis[V]) 
            DFS(G, V);
}
```

### BFS

``` c++
int inq[MAXN];
BFS(Graph G, Vertex S)
{
	queue Q;
	Vertex U, V;
	
	Q.push(S);
	inq[S] = true;
	
	while(!Q.empty()) {
		U = Q.top();
		Q.pop();
        Do(U);
		for(U的每一个邻接点V)
			if(inq[V] == false) {
                Q.push(V);
                inq[V] = true;
            }
	}
} 
/* 
	时间复杂度
	邻接矩阵 O(V^2) //稠密图这个好
	邻接表 O(V + E) //稀疏图当推邻接表
*/

```



### Shortest Path

- Single Source

  Dijkstra 

  - Time complex : O(V ^ 2) (regardless of Data Structures using Matrix or Adjlist) O(ElgV) via Heap
  - Application: |V| ≈ 1e3 - 1e4

  ``` c++
  void Dijkstra(Graph G, int d[], Vertex S)
  {
      fill(d, d + MAXV, INF);
      fill(vis, vis+MAXV, false);
      
      d[s] = 0;
      
      for(int i=0; i<n; i++) {
          int u = -1, MIN = INF;
          for(int j=0; j<n; j++) {
              if(vis[j] == false && d[j] < MIN) {
              	u = j;
              	MIN = d[j];
  			}
          }
          
          if(u == -1) return;
  	    vis[u] = true;
  	    
  	    for(从u出发能到达的所有顶点v) {
  	    	if(!vis[v] && d[u] + weight(u, v) < d[v]) {
  	    		d[v] = d[u] + weight(u, v); 
  			} 
  		}
  	}
  }
  ```

  - 第二标尺

    - 新增边权

      ``` c++
      int cost[MAXV][MAXV];
      int c[MAXV] = {INF};
      c[src] = 0;
      
      for(v = 0; v<MAXV; v++) {
          if(G[u][v] != INF && !vis[v] ) {
              if(d[u] + G[u][v] < d[v]) {
                  d[v] = d[u] + G[u][v];
                  c[v] = c[u] + cost[u][v];
              } else if(d[u] + G[u][v] == d[v] && c[u] + cost[u][v] < c[v]) {
                  c[v] = c[u] + cost[u][v];
              }
          }
      }
      
      ```

    - 新增点权

      ``` c++
      int weight[MAXV];
      int w[MAXV] = {0};
      w[src] = weight[src];
      
      for(v = 0; v<MAXV; v++) {
          if(G[u][v] != INF && !vis[v] ) {
              if(d[u] + G[u][v] < d[v]) {
                  d[v] = d[u] + G[u][v];
                  w[v] = w[u] + weight[v];
              } else if(d[u] + G[u][v] == d[v] && w[u] + weight[v] > w[v]) {
                  w[v] = w[u] + weight[v];
              }
          }
      }
      ```

    - 求最短路径条数

      ``` C++
      int num[MAXV] = {0};
      num[src] = 1;
      
      
      for(v = 0; v<MAXV; v++) {
          if(G[u][v] != INF && !vis[v] ) {
              if(d[u] + G[u][v] < d[v]) {
                  d[v] = d[u] + G[u][v];
                  num[v] = num[u];
              } else if(d[u] + G[u][v] == d[v]) {
                  num[v] += num[u];
              }
          }
      }
      ```

      

- negative edge weight

  - Bellman O(VE)

  ``` c
  bool Bellman(Graph G) {
      for(i=0; i<n-1; i++) {
          for(each edge u->v) {
              if（d[u] + weight(u->v) < d[v]) {
                  d[v] = d[u] + weight(u->v);
              }
          }
  	}
  	// 判断negetive cycle是否存在
      for(each edge u->v) {
          if（d[u] + weight(u->v) < d[v]) {
              return false;
          }
      }
      return true;
  }
  
  ```

  - SPFA O(kE)

  ``` c
  queue<int> Q;
  Q.push(s);
  while(!Q.empty()) {
      u = Q.front();
      Q.pop();
      for(u的所有邻接边u->v) {
          if(d[u] + dis < d[v]) {
              d[v] = d[u] + dis;
              if(v不在队列中) {
                  Q.push(v);
                  if(v入队次数大于n-1) {
                      return false;
                  }
              }
          }
      }
  }
  ```

  

  

- Multisource 

  Floyd 

  - Time Complex : O(n^3)
  - Application : |V| <= 200

  ``` c++
  void Floyd()
  {
  	for(int k=0; k<N; k++) {
  		for(int i=0; i<N; i++) {
  			for(int j=0; j<N; j++) {
  				if(dis[i][k] + dis[k][j] < dis[i][j]) {
  					dis[i][j] = dis[i][k] + dis[k][j];
  				}
  			}
  		}
  	}
  }
  /* 
  	**init**
  	1. fill(dis[0], dis[0] + MAXV * MAXV, INF);
  	2. dis[i][j] = w , which is the direct edge weight in Graph
  	3. dis[i][i] = 0
  	
  	**reconstruct a path**
  	1. p is a predecessor matrix
  	2. p[i][j] should be initialized to i.
  	3. every improvement follows 
  		dis[i][j] = dis[i][k] + dis[k][j];
      	p[i][j] = p[k][j];
  */
  
  Path Reconstruction
  for (k=0;k<n;k++)
    for (i=0;i<n;i++)
      for (j=0;j<n;j++)
        if (d[i][k] + d[k][j] < d[i][j]) {
          d[i][j] = d[i][k]+d[k][j];
          p[i][j] = p[k][j];
        }
  
  // Recursive function to reconstruct the shortest path from i to j
  // using the predecessor matrix computed above
  print_path (int i, int j) {
    if (i!=j)
      print_path(i,p[i][j]);
    print(j);
  }
  
  // 算法笔记 P399
  ```

### 查找

1. 二分查找

   - 满足某条件的元素是否存在？

     ``` c++
     int binarySearch(int A[], int left, int right, int x)
     {
         int mid;
         while(left <= right) {
             mid = (left + right) / 2;
             if(A[mid] == x) return mid;
             else if(A[mid] > x) {
                 right = mid - 1;
             } else {
                 left = mid + 1;
             }
         }
         return -1;
     }
     ```

   - 寻找有序序列第一个满足某条件的元素的位置
   
     从左到右某条件先不满足，而后出现第一个满足某条件的位置。
   
     tips: 第一个不满足某条件C的元素的位置Pos == 第一个满足条件！C的元素位置Pos； 即从左到右（0-Pos-1)均满足C。
   
     ```c++
     int solve(int left, int right) {
     	  int mid;
         while(left < right) {
             mid = (left + right) / 2;
             if(条件成立) {
           		right = mid;
         		} else {
           		left = mid + 1;
         		}
       	}
       	return left;
     }
     ```
   
     ``` c
     // A[] 递增， x为待查询数， 函数返回第一个大于等于x的位置
     // 二分上界为左闭右闭的[left, right], 初值为[0,n]
     int lower_bound(int A[], int left, int right, int x) {
         while (left < right) {
             int mid = (left + right) / 2;
             if (A[mid] >= x) {
                 right = mid;
             } else {
                 left = mid + 1;
             }
         }
         return left;
     }
     ```



​			`upper_bound`与其同理。

2. 二叉查找树

   算法笔记P310

   ``` c++
   // 删除操作
   void deleteBST(Tree &root, int data) {
       if(root == NULL) {
           return;
       }
       if(data > root->data) {
           deleteBST(root->right, data);
       } else if(data < root->data) {
           deleteBST(root->left, data);
       } else {
           if(root->left and root->right) {
               Tree T = findMax(root->left);
               root->data = T->data;
               deleteBST(root->left, root->data);
               
               
           } else {
               if(!root->left) {
                   Tree tmp = root;
                   root = root->right;
                   delete(tmp);
               } else {
                   Tree tmp = root;
                   root = root->left;
                   delete(tmp);
               }
           }
           
       } // else
    
       
   }
   ```

### 排序

快速排序随机主元生成方法：









### Trick and details

见OneNote 上 总结

### 其他小知识点

#### priority_queue 内元素优先级的设置

P223 算法笔记

1. 基本数据类型

​	 最小的元素放前面

``` c++
priority_queue<int, vector<int>, greater<int> >q;
```

2. 结构体数据类型

   重载小于号

   ​        price 小的放前面

``` c++
struct fruit {
    string name;
    int price;
    friend bool operator < (const fruit &f1, const fruit &f2) {
      	return f1.price > f2.price; //小顶推
    }
}
priority_queue<fruit> q;
```

3. 通用

``` c
// int
struct cmp {
    bool operator () (int a, int b) {
        return a > b; // 小顶堆
    }
};
priority_queue<int, vector<int>,  cmp > q;


// struct
struct cmp {
    bool operator () (const fruit &f1, const fruit &f2) {
        return f1.price > f2.price; // price小的在上面
    }
};
priority_queue<fruit, vector<fruit>, cmp> q;

```











#### cin 时间复杂度

cin >>string 1e5没问题