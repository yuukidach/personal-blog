---
title: "最小生成树算法"
date: 2021-01-20T16:08:26+08:00
draft: False
categories: [Learning, Algorithm]
---

## Kruskal 算法

1. 新建图$G$，$G$中拥有原图中相同的节点，但没有边
2. 将原图中所有的边按权值从小到大排序
3. 从权值最小的边开始，如果这条边连接的两个节点于图$G$中不在同一个连通分量中，则添加这条边到图$G$中
4. 重复3，直至图$G$中所有的节点都在同一个连通分量中

伪代码：

``` shell
algorithm Kruskal(G) is
    F:= ∅
    for each v ∈ G.V do
        MAKE-SET(v)
    for each (u, v) in G.E ordered by weight(u, v), increasing do
        if FIND-SET(u) ≠ FIND-SET(v) then
            F:= F ∪ {(u, v)}
            UNION(FIND-SET(u), FIND-SET(v))
    return F
```

## Prim 算法

1. 输入：一个加权连通图，其中顶点集合为$V$，边集合为$V$；
2. 初始化：$V_{new} = {x}$，其中$x$为集合$V$中的任一节点（起始点），$E_{new}=\{\}$
3. 重复下列操作，直到$V_{new} = V$：
    a. 在集合$E$中选取权值最小的边$(u, v)$，其中$u$为集合$V_{new}$中的元素，而$v$则是$V$中没有加入$V_{new}$的顶点（如果存在有多条满足前述条件即具有相同权值的边，则可任意选取其中之一）；
    b. 将$v$加入集合$V_{new}$中，将$(u,v)$加入集合$E_{new}$中；
4. 输出：使用集合$V_{new}$和$E_{new}$来描述所得到的最小生成树。

Leetcode例题：[1584. Min Cost to Connect All Points](https://leetcode-cn.com/problems/min-cost-to-connect-all-points/)

参考: [最小生成树](https://oi-wiki.org/graph/mst/)