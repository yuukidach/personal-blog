---
title: "Floyd-Warshall 算法"
date: 2021-01-07T20:44:45+08:00
draft: False
categories: [Learning, Algorithm]
---

**弗洛伊德算法（Floyd-Warshall algorithm)** 是解决两点间最短路径的一种算法。时间复杂度$O(N^{3})$，空间复杂度$O(N^{2})$。

其算法原理为动态规划：

设$D_{i,j,k}$为从i到j的只以 (1...k) 集合中的节点为中间节点的最短路径长

1. 若最短路径经过k,则$D_{i,j,k}=D_{i,k,k-1}+D_{j,k,k-1}$
2. 若不经过k,则$D_{i,j,k}=D_{i,j,k-1}$

因而，$D_{i,j,k}=min(D_{i,k,k-1}+D_{j,k,k-1}, D_{i,j,k-1})$

伪代码：

``` shell
1 let dist be a |V| × |V| array of minimum distances initialized to ∞ (infinity)
2 for each vertex v
3    dist[v][v] ← 0
4 for each edge (u,v)
5    dist[u][v] ← w(u,v)  // the weight of the edge (u,v)
6 for k from 1 to |V|
7    for i from 1 to |V|
8       for j from 1 to |V|
9          if dist[i][j] > dist[i][k] + dist[k][j] 
10             dist[i][j] ← dist[i][k] + dist[k][j]
11         end if
```

例题可参照[Leecode 399](https://leetcode-cn.com/problems/evaluate-division/)。