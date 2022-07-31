---
title: 【HDU-1532】 Drainage Ditches（dinic最大流入门题）
mathjax: true
copyright: true
date: 2019-08-10 15:12:37
tags: [网络流,最大流,板子]
categories: 题解
---
# 描述
传送门：[HDU-532](http://acm.hdu.edu.cn/showproblem.php?pid=1532)

>&emsp;Every time it rains on Farmer John's fields, a pond forms over Bessie's favorite clover patch. This means that the clover is covered by water for awhile and takes quite a long time to regrow. Thus, Farmer John has built a set of drainage ditches so that Bessie's clover patch is never covered in water. Instead, the water is drained to a nearby stream. Being an ace engineer, Farmer John has also installed regulators at the beginning of each ditch, so he can control at what rate water flows into that ditch. 
Farmer John knows not only how many gallons of water each ditch can transport per minute but also the exact layout of the ditches, which feed out of the pond and into each other and stream in a potentially complex network. 
Given all this information, determine the maximum rate at which water can be transported out of the pond and into the stream. For any given ditch, water flows in only one direction, but there might be a way that water can flow in a circle. 

<!--more-->
## Input
> The input includes several cases. For each case, the first line contains two space-separated integers, $N (0 <= N <= 200)$ and $M (2 <= M <= 200)$. N is the number of ditches that Farmer John has dug. M is the number of intersections points for those ditches. Intersection 1 is the pond. Intersection point M is the stream. Each of the following N lines contains three integers, Si, Ei, and $C_i$. $S_i$ and $E_i (1 <= S_i, E_i <= M)$ designate the intersections between which this ditch flows. Water will flow through this ditch from $S_i$ to $E_i$. $C_i$ (0 <= C_i <= 10,000,000)$ is the maximum rate at which water will flow through the ditch.

## Output
> For each case, output a single integer, the maximum rate at which water may emptied from the pond. 

## Examples
* intput
```c++
5 4
1 2 40
1 4 20
2 4 20
2 3 30
3 4 10
```
* output
```c++
50
```

# 大致题意
> 给你一个图，求最大流。

# 思路
>* 就是求最大流的板子题，用dinic算法。
>* 每次先用bfs对图进行分层，然后dfs找一条从s（源点）到t（汇点）的通路，再将这条路上的所有边的流量加上这条路上所有边中容量最小的值。

# 代码
```c++
#include<iostream>
#include<stdio.h>
#include<string.h>
#include<queue>
#include<cmath>
using namespace std;
#define rep(i,a,n) for(int i=a;i<=n;i++)
#define repd(i,a,n) for(int i=n;i>=a;i--)
#define CRL(a,x) memset(a,x,sizeof(a))
#define inf 0x3f3f3f3f
typedef long long ll;
const int N=2e2+5;
const double Pi = acos(-1);
const double e = exp(1.0);
const int mod=1e9+7;

int Begin[N],d[N],s,t,cnt,n,m,cur[N];

struct node{int to,c,f,nex;}Edge[N<<1];      //链式前向星存图，根据题意开点的两倍的边 c:容量，f:流量，一次存两条边，i^1和i就是一队正反边
void add(int u,int v,int w){               //存u->v的正反边，反边的流量和容量都是0
    Edge[++cnt]=(node){v,w,0,Begin[u]};
    Begin[u]=cnt;
    Edge[++cnt]=(node){u,0,0,Begin[v]};
    Begin[v]=cnt;
}


bool bfs(){             //对图分层
    queue<int> Q;Q.push(s);
    CRL(d,0);d[s]=1;    //s是第一层
    while(!Q.empty()){
        int u=Q.front();Q.pop();
        for(int  i=Begin[u];~i;i=Edge[i].nex){  //遍历一个点的所有边
            int v=Edge[i].to;
            if(!d[v]&&Edge[i].c>Edge[i].f){     //如果这条边指向的点没有被访问过且这条边流量还没满。
                d[v]=d[u]+1;
                Q.push(v);
                if(v==t) return true;           //如果到达了t，说明有通路。
            }
        }
    }
    return false;   //无通路
}

int dfs(int u,int flow){        //找一条从s->u的通路
    if(u==t) return flow;       //找到了，返回这条条路的最大流量
    for(int& i=cur[u];~i;i=Edge[i].nex){    //遍历这个点出发的所有边，cur:当前弧优化，之前找过的边就不找了
        int v=Edge[i].to,tflow;
        if(d[v]==d[u]+1&&Edge[i].c>Edge[i].f){ //如果是下一层的，且边的流量还没满
            tflow=dfs(v,min(Edge[i].c-Edge[i].f,flow));     //取小地，继续dfs
            if(tflow){              //找到了一条通路
                Edge[i].f+=tflow;   //更新流量
                Edge[i^1].f-=tflow;
                return tflow;       //返回流量
            }
        }
    }
    return 0;
}

int dinic(){
    int flow=0,tem;
    while(bfs()){   //分层
        rep(i,1,n) cur[i]=Begin[i];     //重置当前弧
        while(tem=dfs(s ,inf))          //dfs找一条通路
            flow+=tem;       //找到了就加流量
    }
    return flow;
}

int main(){
    while(~scanf("%d%d",&m,&n)){
        cnt=-1;s=1;t=n;
        CRL(Begin,-1);
        int x,y,w;
        rep(i,1,m){
            scanf("%d%d%d",&x,&y,&w);
            add(x,y,w);
        }
        printf("%d\n",dinic());
    }
    return 0;
}

```
