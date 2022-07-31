---
title: Nearest Common Ancestors（Tarjan离线+LCA模板题）
mathjax: true
copyright: true
date: 2018-08-09 17:02:53
tags: [Tarjan,图论,LCA,离线]
categories: 题解
---
# 描述
传送门：[poj-1330](http://poj.org/problem?id=1330)

>&emsp;A rooted tree is a well-known data structure in computer science and engineering. An example is shown below: ![图](http://poj.org/images/1330_1.jpg)

<!--more-->

> In the figure, each node is labeled with an integer from {1, 2,...,16}. Node 8 is the root of the tree. Node x is an ancestor of node y if node x is in the path between the root and node y. For example, node 4 is an ancestor of node 16. Node 10 is also an ancestor of node 16. As a matter of fact, nodes 8, 4, 10, and 16 are the ancestors of node 16. Remember that a node is an ancestor of itself. Nodes 8, 4, 6, and 7 are the ancestors of node 7. A node x is called a common ancestor of two different nodes y and z if node x is an ancestor of node y and an ancestor of node z. Thus, nodes 8 and 4 are the common ancestors of nodes 16 and 7. A node x is called the nearest common ancestor of nodes y and z if x is a common ancestor of y and z and nearest to y and z among their common ancestors. Hence, the nearest common ancestor of nodes 16 and 7 is node 4. Node 4 is nearer to nodes 16 and 7 than node 8 is. 

> For other examples, the nearest common ancestor of nodes 2 and 3 is node 10, the nearest common ancestor of nodes 6 and 13 is node 8, and the nearest common ancestor of nodes 4 and 12 is node 4. In the last example, if y is an ancestor of z, then the nearest common ancestor of y and z is y. 

> Write a program that finds the nearest common ancestor of two distinct nodes in a tree. 

## Input
> The input consists of T test cases. The number of test cases **T** is given in the first line of the input file. Each test case starts with a line containing an integer N , the number of nodes in a tree, $2<=N<=10,000$. The nodes are labeled with integers $1, 2,..., N$. Each of the next $N -1$ lines contains a pair of integers that represent an edge --the first integer is the parent node of the second integer. Note that a tree with N nodes has exactly $N - 1$ edges. The last line of each test case contains two distinct integers whose nearest common ancestor is to be computed.

## Output
> Print exactly one line for each test case. The line should contain the integer that is the nearest common ancestor.

## Examples
* intput
```c++
2
16
1 14
8 5
10 16
5 9
4 6
8 4
4 10
1 13
6 15
10 11
6 7
10 2
16 3
8 1
16 12
16 7
5
2 3
3 4
3 1
1 5
3 5
```
* output
```c++
4
3
```

# 题意
> 第二段是对[LCA](https://baike.baidu.com/item/%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88/8918834?fr=aladdin)的解释，然后每组数据给定有N个节点，N-1条边，最后给出一组u,v，求u,V的最近公共祖先。

# 伪代码
```C++
Tarjan(u){//Union和find为并查集合并函数和查找函数
        for each(u,v){    //访问所有u子节点v
        Tarjan(v);        //继续往下遍历
        Union(u,v);    //合并v到u上
        标记v被访问过;
    }
    for each(u,e){    //访问所有和u有询问关系的e
        如果e被访问过;
        u,e的最近公共祖先为find(e);
    }
}
```

# 代码
```c++
/*Problem: 1330     Memory: 1668K     Time: 250MS     Language: G++     Result: Accepted*/
#include<iostream>
#include<vector>
#include<string.h>
using namespace std;
#define rep(i,a,n) for(int i=a;i<n;i++)
#define repd(i,a,n) for(int i=n-1;i>=a;i--)
#define CRL(a,x) memset(a,x,sizeof(a))
const int N=1e4+5;

int Fa[N],u,v;vector<int> Map[N];bool Vis[N];
int Find(int x){return Fa[x]==x? x:Fa[x]=Find(Fa[x]);}
void Union(int a,int b){int fa=Find(a),fb=Find(b);Fa[fa]=fb;}

void Init(){
    rep(i,0,N) {Fa[i]=i;Map[i].clear();}
    CRL(Vis,false);
}

bool Tarjan(int root){
    rep(i,0,Map[root].size()){
        if(Tarjan(Map[root][i]))return true;
        Union(Map[root][i],root);
        Vis[Map[root][i]]=true;
    }
    if(root==u&&Vis[v]){
        cout<< Find(v)<<endl;
        return true;
    }
    else if(root==v&&Vis[u]){
        cout<< Find(u)<<endl;
        return true;
    }
    return false;
}

int main()
{
    std::ios::sync_with_stdio(false);
    int n,m,_,a,b,root;
    cin>>_;
    while(_--){
        Init();
        cin>>n;
        rep(i,0,n-1){
            cin>>a>>b;
            Fa[b]=a;
            Map[a].push_back(b);
        }

        root=Find(a);       //找根
        cin>>u>>v;

        rep(i,0,N) Fa[i]=i;
        Tarjan(root);
    }

    return 0;
}
```
