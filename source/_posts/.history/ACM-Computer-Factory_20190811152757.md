---
title: 【poj-343】 ACM Computer Factory （最大流记录路径）
mathjax: true
copyright: true
date: 2019-08-11 02:12:05
tags: [网络流,最大流]
categories: 题解
---
# 描述
传送门：[poj-3436](http://poj.org/problem?id=3436)

>&emsp;As you know, all the computers used for ACM contests must be identical, so the participants compete on equal terms. That is why all these computers are historically produced at the same factory.

> Every ACM computer consists of P parts. When all these parts are present, the computer is ready and can be shipped to one of the numerous ACM contests.

<!--more-->
> Computer manufacturing is fully automated by using N various machines. Each machine removes some parts from a half-finished computer and adds some new parts (removing of parts is sometimes necessary as the parts cannot be added to a computer in arbitrary order). Each machine is described by its performance (measured in computers per hour), input and output specification.

> Input specification describes which parts must be present in a half-finished computer for the machine to be able to operate on it. The specification is a set of P numbers 0, 1 or 2 (one number for each part), where 0 means that corresponding part must not be present, 1 — the part is required, 2 — presence of the part doesn't matter.

> Output specification describes the result of the operation, and is a set of P numbers 0 or 1, where 0 means that the part is absent, 1 — the part is present.

> The machines are connected by very fast production lines so that delivery time is negligibly small compared to production time.

> After many years of operation the overall performance of the ACM Computer Factory became insufficient for satisfying the growing contest needs. That is why ACM directorate decided to upgrade the factory.

> As different machines were installed in different time periods, they were often not optimally connected to the existing factory machines. It was noted that the easiest way to upgrade the factory is to rearrange production lines. ACM directorate decided to entrust you with solving this problem.

## Input
> Input file contains integers $P\ N$, then $N$ descriptions of the machines. The description of ith machine is represented as by $2 P + 1$ integers $Q_i\ S_{i,1}\ S_{i,2}\ ...\ S_{i,P}\ D_{i,1}\ D_{i,2}\ ...D_{i,P}$, where $Q_i$ specifies performance, $S_{i,j}$ — input specification for part $j, D_{i,k}$ — output specification for part $k$.

## Constraints

$1\ ≤\ P\ ≤\ 10,\ 1\ ≤\ N\ ≤\ 50,\ 1\ ≤\ Q_i\ ≤\ 10000$

## Output
> Output the maximum possible overall performance, then $M$ — number of connections that must be made, then $M$ descriptions of the connections. Each connection between machines $A$ and $B$ must be described by three positive numbers $A\ B\ W$, where $W$ is the number of computers delivered from $A$ to $B$ per hour.

> If several solutions exist, output any of them.

## Examples
* intput
```c++
Sample input 1
3 4
15  0 0 0  0 1 0
10  0 0 0  0 1 1
30  0 1 2  1 1 1
3   0 2 1  1 1 1
Sample input 2
3 5
5   0 0 0  0 1 0
100 0 1 0  1 0 1
3   0 1 0  1 1 0
1   1 0 1  1 1 0
300 1 1 2  1 1 1
Sample input 3
2 2
100  0 0  1 0
200  0 1  1 1
```
* output
```c++
Sample output 1
25 2
1 3 15
2 3 10
Sample output 2
4 5
1 3 3
3 5 3
1 2 1
2 4 1
4 5 1
Sample output 3
0 0
```

# 大致题意
> 要生产一种产品，产品有P种零件，共有N台机器，输入的每一行第一个数字代表这台机器的产量，2-1+P个数字表示这个机器能处理的半成品状态，1表示需要这个零件，0表示不需要这个零件，2表示这个零件可有可无。 后P个表示这个机器产出的产品状态，1表示有这个零件，0表示没有零件。
求组装好的成产线的最大工作效率（每小时最多生成多少成品，成品的定义就是所有部分的状态都是"1"）

# 思路
>* 如果一个机器的产出和另一个机器的需求一样的话，就可以连一条边，如果一个机器输入的的零件没有1的话，就和s连一条边，如果输出全为1（即成品），就与t连一条边。跑最大流。
>* 由于每个机器输出是有限的，所以还是一样通过拆点来加以限制，对于每个机器 i都拆成 i，i'两个节点，再连一条权值为产量的边。
>* 这道题需要记录路径，在跑完最大流之后，对所有边判断一下是否有流量。还要排除原本没有的点，比如链接到s和t的边、拆点之后i和i'之间的边。

# 代码
```c++
#include<iostream>
#include<stdio.h>
#include<queue>
#include<vector>
#include<string.h>
#include<cmath>
using namespace std;
#define rep(i,a,n) for(int i=a;i<=n;i++)
#define CRL(a,x) memset(a,x,sizeof(a))
#define inf 0x3f3f3f3f
typedef long long ll;
const int N=1e2+5;
const int M=1e4+5;

int d[N],Begin[N],s,t,cnt,p,n;

struct node1{
    int w,q[15],h[15];
}lin[100];

struct node{int to,c,nex,f;}Edge[M];
struct nodeans{int u,v,w;};

void add(int u,int v,int w){
    Edge[++cnt]=(node){v,w,Begin[u],0};
    Begin[u]=cnt;
    Edge[++cnt]=(node){u,0,Begin[v],0};
    Begin[v]=cnt;
}

bool bfs(){
    CRL(d,0);d[s]=1;
    queue<int>Q;Q.push(s);
    while(!Q.empty()){
        int u=Q.front();Q.pop();
        for(int i=Begin[u];~i;i=Edge[i].nex){
            int v=Edge[i].to;
            if(Edge[i].c>Edge[i].f&&!d[v]){
                d[v]=d[u]+1;
                Q.push(v);
                if(v==t) return true;
            }
        }
    }
    return false;
}

int dfs(int u,int flow){
    if(u==t) return flow;
    for(int i=Begin[u];~i;i=Edge[i].nex){
        int v=Edge[i].to;
        if(Edge[i].c>Edge[i].f&&d[u]+1==d[v]){
            int tflow=dfs(v,min(flow,Edge[i].c-Edge[i].f));
            if(tflow){
                Edge[i].f+=tflow;
                Edge[i^1].f-=tflow;
                return tflow;
            }
        }
    }
    return 0;
}

int dinic(){
    int flow=0,tflow;
    while(bfs()){
        while(tflow=dfs(s,inf))
            flow+=tflow;
    }
    return flow;
}

void Buil(){
    cnt=-1;
    CRL(Begin,-1);
    s=0;t=2*n+1;
    rep(i,1,n){
        scanf("%d",&lin[i].w);
        rep(j,1,p) scanf("%d",&lin[i].q[j]);
        rep(j,1,p) scanf("%d",&lin[i].h[j]);
    }

    rep(i,1,n){
        add(i,i+n,lin[i].w);
        int flag=1;
        rep(j,1,p) if(lin[i].q[j]==1) flag=0;
        if(flag) add(s,i,inf);

        flag = 1;
        rep(j,1,p) if(lin[i].h[j]!=1) flag=0;
        if(flag) add(i+n,t,inf);

        rep(j,1,n){
            if(i==j) continue;
            rep(k,1,p){
                if((lin[i].h[k]^lin[j].q[k])==1) break;  //一个为0一个为1就break
                if(k==p) add(i+n,j,inf);
            }
        }
    }
}

int main(){
    scanf("%d%d",&p,&n);
    Buil();
    printf("%d ",dinic());

    vector<nodeans> ans;
    rep(i,0,cnt){
        if(Edge[i].f<=0||Edge[i^1].to==s||Edge[i].to==t||Edge[i].to-Edge[i^1].to==n) continue;
        int t=Edge[i^1].to;
        if(t>n) t-=n;
        ans.push_back(nodeans{t,Edge[i].to,Edge[i].f});
    }
    printf("%d\n",ans.size());

    int len=ans.size();
    rep(i,0,len-1)
        printf("%d %d %d\n",ans[i].u,ans[i].v,ans[i].w);

    return 0;
}

```
