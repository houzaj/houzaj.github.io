---
layout: post
title: '蒟蒻的算法DOC - 网络流 · ISAP算法'
date: 2019-07-15
author: HouZAJ
cover: 'https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190715%20ALG-DOC-ISAP/main-01.png'
tags: Algorithm DOC
---

> 本文介绍了网络流中最大流求解算法中的ISAP算法，包括算法过程、算法正确性等  

<br>

### BB
这个学期在做算法实验的时候，顺便回顾了一下ISAP算法，然后发现网上的理解其实不太对，故写此文  
应该会有许多不严谨的地方 \_(:з)∠)\_  
<br>

### Reference
- https://www.cnblogs.com/nervendnig/p/8927773.html  
- Ahuja, R. K., Mehlhorn, K., Orlin, J., & Tarjan, R. E. (1990). Faster algorithms for the shortest path problem. *Journal of the ACM (JACM), 37*(2), 213-223.  

<br>

### Introduction
网络流求最大流的最经典方法当属Ford-Fulkerson算法，其是增广路算法，思路简单实现容易，但是时间复杂度为`O(f*M)`，与最大流有关。后来人们发现，当沿最短路增广时，时间复杂度与最大流无关，这就是最短路径增广算法（Shortest Path Augmenting Algorithm，简称SAP）。SAP算法是一类算法，包括了Edmonds-Karp算法（EK算法）、Dinic算法、Improved Shortest Augmenting Path算法（ISAP算法）。其中EK算法的时间复杂度为`O(NM^2)`，Dinic与ISAP算法的时间复杂度为`O(MN^2)`。虽然Dinic与ISAP算法时间复杂度相同，但是ISAP算法常数更小，效率更高。  
<br>

### Definition
在介绍算法实现前，首先引入一些定义。  
- **距离函数（distance function）** `d(u)`：一个函数。称距离函数合法（validity）当其在残余网络（redisual network）中满足`d(t)=0, d(u)<=d(v)+1 for each arc(u, v)`。  
- **允许弧（admissible arc）** `arc(u, v)`：在残余网络中满足`d(u) = d(v) + 1`的弧`arc(u, v)`。  

<br>

### Implentation
下面介绍算法的实现过程。  
1. **初始化**：令当前点`u`为`s`，即`u=t`。转2。  
2. **反向BFS**：进行反向BFS，即从汇点`t`出发，令`d(t)=0`，后进行BFS，令`d(u)=d(v)+1`，进行下一步。（此步可以不进行，直接进行下一步。）  
3. **判断是否满足`d(u) >= n`**：若`d(u) >= n`，则此时求得的流量为最大流，结束算法。  
4. **判断是否是汇点`t`**: 对当前点`u`判断是否为汇点`t`，是则进行增广，转8，执行 **AUGMENT操作**。  
5. **判断允许弧是否存在**：对当前点`u`扫描邻接边，若`arc(u, v)`为允许弧，则转6，执行 **ADVANCE操作**；否则，转7，执行 **RETREAT操作**。    
6. **ADVANCE操作**：此时找到的允许弧为`arc(u, v)`，则令`u=v, pre(v)=u`，此处的`pre`是记录`v`的前驱节点。转3。  
7. **RETREAT操作**：此时找不到允许弧，则令`d(u) = min { d(v) + 1, arc(u, v) \in Ef }`，即令`d(u)`为在残余网络中`u`的邻接边`(u, v)`中`d(v)`的最小值+1，称这一操作为 **重标号操作（relabel）**。后令`u=pre(u)`，转3。  
8. **AUGMENT操作**：沿前驱节点找到到达汇点`t`的路径，然后沿这条路径增广，累加流量。后令`u=s`，转5。  

```cpp
static int prev[N];     //前驱节点
static int preEdge[N];  //前驱边
static int d[N];        //距离函数

inline int ISAP(int src, int des, int n) {
    //初始化部分
    memset(d, 0, sizeof(d));
    prev[src] = src;
    int u = src;
    int ans = 0;
    //判断是否满足d[u] >= n，满足则结束算法
    while(d[u] < n) {
        // 判断是否达到汇点t
        if(u == des) {
            // AUGMENT操作，增广后回到源点s，累加答案
            int aug = inf, v;
            for(u = prev[des], v = des; v != src; v = u, u = prev[u]) {
                aug = min(aug, e[preEdge[v]].w);
            }
            for(u = prev[des], v = des; v != src; v = u, u = prev[u]) {
                e[preEdge[v]].w -= aug;
                e[preEdge[v]^1].w += aug;
            }
            ans += aug;
        }

        // 判断允许弧是否存在，存在则执行ADVANCE操作
        bool canAdvance = false;
        for(int i = head[u]; ~i; i = e[i].nxt) {
            int v = e[i].v;
            if(e[i].w && d[u] == d[v] + 1) {
                prev[v] = u;
                preEdge[v] = i;
                u = v;
                canAdvance = true;
                break;
            }
        }

        //不存在则进行 RETREAT操作
        if(!canAdvance) {
            int mind = n;
            for(int i = head[u]; ~i; i = e[i].nxt){
                if(e[i].w) {
                    mind = min(mind, d[e[i].v]);
                }
            }
            d[u] = mind + 1;
            u = prev[u];
        }
    }
    return ans;
}
```
<br>

### Correction
对于上述定理，有如下定理或性质。由于公式较多，这部分采用贴图呈现。  
![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190715%20ALG-DOC-ISAP/1.png)  
![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190715%20ALG-DOC-ISAP/2.png)  
![](https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190715%20ALG-DOC-ISAP/3.png)  
综上，算法的正确性与时间复杂度正确性等得以证明。  
<br>

### Optimization
实践证明，上述算法其实还是比较慢的。发明者还提出了 **GAP优化**。  
GAP优化是指使用数组`gap`维护`gap[d(u)]`，然后中间有一处`gap[i]==0`，那么无解，此时取得的流量为最大流。换句话说，在进行`RETREAT操作`时，若当前`d(u)`是整个图中唯一的一个，则可以提前结束算法，此时得到的流量为最大流。可以通过构造切割，证明其违反了`d(u) <= d(v) + 1`证明。   
<br>
