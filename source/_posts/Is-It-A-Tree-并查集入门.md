---
title: Is It A Tree? (并查集入门)
mathjax: true
copyright: true
date: 2018-03-12 08:29:59
tags: 并查集
categories: 题解
---
# 描述
传送门：[poj-1308](http://poj.org/problem?id=1308)

>&emsp;A tree is a well-known data structure that is either empty (null, void, nothing) or is a set of one or more nodes connected by directed edges between nodes satisfying the following properties. 
>&emsp;There is exactly one node, called the root, to which no directed edges point. 
Every node except the root has exactly one edge pointing to it.
>&emsp;There is a unique sequence of directed edges from the root to each node. 
>&emsp;For example, consider the illustrations below, in which nodes are represented by circles and edges are represented by lines with arrowheads. The first two of these are trees, but the last is not. 
<center>![图](http://poj.org/images/1308_1.jpg)

>&emsp;In this problem you will be given several descriptions of collections of nodes connected by directed edges. For each of these you are to determine if the collection satisfies the definition of a tree or not.

<!--more-->
## Input
>&emsp;The input will consist of a sequence of descriptions (test cases) followed by a pair of negative integers. Each test case will consist of a sequence of edge descriptions followed by a pair of zeroes Each edge description will consist of a pair of integers; the first integer identifies the node from which the edge begins, and the second integer identifies the node to which the edge is directed. Node numbers will always be greater than zero.

## Output
>&emsp;For each test case display the line "Case k is a tree." or the line "Case k is not a tree.", where k corresponds to the test case number (they are sequentially numbered starting with 1).

## Examples
* intput
```
6 8  5 3  5 2  6 4
5 6  0 0

8 1  7 3  6 2  8 9  7 5
7 4  7 8  7 6  0 0

3 8  6 8  6 4
5 3  5 6  5 2  0 0
-1 -1
```
* output
```
Case 1 is a tree.
Case 2 is a tree.
Case 3 is not a tree.
```

# 思路
>* 初始化每个点单独为一个集合，每次读入A,B就合并A,B所在的集合，每个集合都有一个**代表元素**，集合中的元素都指向代表元素并且只有代表元素指向自己，如果读入A,B的时候发现A,B的代表数组一样的话就说明有多个节点指向A或B,不是树。
>* 由于节点不一定是从1开始的，如上图的第一个。所以我们用一个mark数组来标记一组数据中“提到”的节点，最后遍历一遍看连通分支数是否等于1。
>* 有一组神坑数据：
0 0 
-1 -1

# 代码
```c++
#include<bits/stdc++.h>
using namespace std;
#define CRL(a) memset(a,0,sizeof(a))
#define MAX 0xfffffff
typedef unsigned long long LL;
typedef  long long ll;
const double Pi = acos(-1);
const double e = 2.718281828459;
const int mod =1e9+7;

int F[1005];

int f(int x) {
    return F[x]==x? x:F[x]=f(F[x]);
}

void Union(int a,int b) {
    a=f(a);
    b=f(b);
    if(a!=b) F[a]=b;
}

int main() {
    int test,n,way,a,b,ans=0;
    cin>>test;
    while(test--) {
        ans=0;
        cin>>n>>way;
        for(int i=0; i<=n; i++) F[i]=i;
        for(int i=0; i<way; i++) {
            cin>>a>>b;
            Union(a,b);
        }

        for(int i=1; i<=n; i++)		//查找有几个代表元素
            if(F[i]==i)
                ans++;

        cout<<ans<<endl;
    }
    return 0;
}

```
