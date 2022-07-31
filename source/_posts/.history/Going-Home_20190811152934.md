---
title: 【poj-2195】 Going Home（二分图带权匹配+最小费用最大流）
mathjax: true
copyright: true
date: 2019-08-11 01:45:15
tags: [网络流,最小费用最大流,二分图]
categories: 题解
---
# 描述
传送门：[poj-2195](http://poj.org/problem?id=2195)

>&emsp;On a grid map there are n little men and n houses. In each unit time, every little man can move one unit step, either horizontally, or vertically, to an adjacent point. For each little man, you need to pay a $1 travel fee for every step he moves, until he enters a house. The task is complicated with the restriction that each house can accommodate only one little man. 

>Your task is to compute the minimum amount of money you need to pay in order to send these n little men into those n different houses. The input is a map of the scenario, a '.' means an empty space, an 'H' represents a house on that point, and am 'm' indicates there is a little man on that point. 
![](http://poj.org/images/2195_1.jpg)

You can think of each point on the grid map as a quite large square, so it can hold n little men at the same time; also, it is okay if a little man steps on a grid with a house without entering that house.
Input
<!--more-->
## Input
> There are one or more test cases in the input. Each case starts with a line giving two integers $N$ and $M$, where $N$ is the number of rows of the map, and $M$ is the number of columns. The rest of the input will be $N$ lines describing the map. You may assume both $N$ and $M$ are between 2 and 100, inclusive. There will be the same number of 'H's and 'm's on the map; and there will be at most 100 houses. Input will terminate with 0 0 for $N$ and $M$.

## Output
> For each test case, output one line with the single integer, which is the minimum amount, in dollars, you need to pay.

## Examples
* intput
```c++
2 2
.m
H.
5 5
HH..m
.....
.....
.....
mm..H
7 8
...H....
...H....
...H....
mmmHmmmm
...H....
...H....
...H....
0 0
```
* output
```c++
2
10
28
```

# 大致题意
> m表示人，H表示房子，每个房子只能进一个人，房子数等于人数。要让所有人到自己的房子的距离和最小，请问这个距离和是多少。$(x_1,y_1)$和$(x_2,y_2)$之间的距离就是$|x_1 - x_2|+|y_1-y_2|$.

# 思路
>* 加入超级源点s和超级汇点t，所有房子和s连一条边，所有人和t连一条边，从s到t跑最小费用最大流。

# 代码
```c++
#include<stdio.h>
#include<queue>
#include<string.h>
using namespace std;
#define rep(i,a,n) for(int i=a;i<=n;i++)
#define repd(i,a,n) for(int i=n;i>=a;i--)
#define CRL(a,x) memset(a,x,sizeof(a))
#define inf 0x3f3f3f3f
typedef long long ll;
const int N=1e3+5;
const int mod=1e9+7;
int abs(int x){return x<0? -x:x;}
int min(int a,int b){return a<b? a:b;}

int inq[N],dis[N],Begin[N],per[N],Flow[N],n,m,cnt;
struct node {int to,c,next,cost,f;}Edge[N*20];

void add(int u,int v,int c,int cost){
    Edge[++cnt]=(node){v,c,Begin[u],cost,0};
    Begin[u]=cnt;
    Edge[++cnt]=(node){u,0,Begin[v],-cost,0};
    Begin[v]=cnt;
}

bool spfa(int s,int t,int &flow,ll &cost){  //SPFA找一条s到t的费用最短路
    rep(i,s,t) dis[i]=inf;
    CRL(inq,0);dis[s]=0;
    per[s] = -1;
    Flow[s] = inf;

    queue<int> q;
    q.push(s);
    while(!q.empty()){
        int u = q.front();
        q.pop(); inq[u] = 0;
        for(int i=Begin[u];~i;i=Edge[i].next){
            int v=Edge[i].to;
            if(Edge[i].c>Edge[i].f&&dis[v]>dis[u]+Edge[i].cost){
                dis[v]=dis[u]+Edge[i].cost;
                Flow[v]=min(Flow[u],Edge[i].c-Edge[i].f);
                per[v]=i;
                if(!inq[v]){
                    q.push(v);
                    inq[v]=1;
                }
            }
        }
    }
    if(dis[t]==inf) return false;
    flow += Flow[t];
    cost += (ll)dis[t]*(ll)Flow[t];     //费用=距离*流量
    for(int i=per[t];~i;i=per[Edge[i^1].to]){   //更新流量
        Edge[i].f+=Flow[t];
        Edge[i^1].f-=Flow[t];
    }
    return true;
}

int MCMF(int s,int t,ll &cost){
    cost =0;
    int flow = 0;
    while(spfa(s,t,flow,cost));
    return flow;
}

void Built(){       //建图
    int H[N][2],M[N][2],cntm=0,cnth=0,s=0,t=201;
    char x;
    rep(i,1,n){
        getchar();
        rep(j,1,m){
            x=getchar();
            if(x=='H') {
                H[++cnth][0]=i;
                H[cnth][1]=j;
            }
            else if(x=='m'){
                M[++cntm][0]=i;
                M[cntm][1]=j;
            }
        }
    }
    rep(i,1,cnth){
        add(s,i,1,0);
        add(i+100,t,1,0);
        rep(j,1,cntm)
            add(i,j+100,1,abs(H[i][0]-M[j][0])+abs(H[i][1]-M[j][1]));
    }
}

int main(){
    ll cost;int s=0,t=201;
    while(scanf("%d%d",&n,&m),n+m){
    	cnt=-1;
        CRL(Begin,-1);
        Built();
        MCMF(s,t,cost);
    	printf("%d\n",cost);
    }

    return 0;
}

```
