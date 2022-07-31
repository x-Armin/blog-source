---
title: Layout（差分约束+链式向前星）
mathjax: true
copyright: true
date: 2018-06-26 01:53:05
tags: [差分约束,图论,最短路,spfa]
categories: 题解
---
# 描述
传送门：[poj-3169](http://poj.org/problem?id=3169)

>&emsp;Like everyone else, cows like to stand close to their friends when queuing for feed. FJ has $N (2\ <=\ N <=\ 1,000)$ cows numbered 1..N standing along a straight line waiting for feed. The cows are standing in the same order as they are numbered, and since they can be rather pushy, it is possible that two or more cows can line up at exactly the same location (that is, if we think of each cow as being located at some coordinate on a number line, then it is possible for two or more cows to share the same coordinate). 

> Some cows like each other and want to be within a certain distance of each other in line. Some really dislike each other and want to be separated by at least a certain distance. A list of $ML\ (1\ <= ML\ <=\ 10,000)$ constraints describes which cows like each other and the maximum distance by which they may be separated; a subsequent list of MD constraints $(1\ < MD\ <=\ 10,000)$ tells which cows dislike each other and the minimum distance by which they must be separated. 

> Your job is to compute, if possible, the maximum possible distance between cow 1 and cow $N$ that satisfies the distance constraints.

<!--more-->
## Input
> Line 1: Three space-separated integers: N, ML, and MD. 

> Lines 2..ML+1: Each line contains three space-separated positive integers: A, B, and D, with $1\ <=\ A\ <\ B\ <= N$. Cows A and B must be at most $D (1\ <=\ D <=\ 1,000,000)$ apart. 

> Lines ML+2..ML+MD+1: Each line contains three space-separated positive integers: A, B, and D, with $1\ <=\ A\ <\ B\ <= N$. Cows A and B must be at least $D (1\ <=\ D\ <=\ 1,000,000)$ apart.

## Output
> Line 1: A single integer. If no line-up is possible, output -1. If cows 1 and $N$ can be arbitrarily far apart, output -2. Otherwise output the greatest possible distance between cows 1 and $N$.

## Examples
* intput
```
4 2 1
1 3 10
2 4 20
2 3 3
```
* output
```
27
```

# 思路
>* 差分约束模板题，a喜欢b,就连一条长为c的边，a不喜欢b就连一条长为-c的边。
>* 用SPFA跑一个最短路。很容易想到，有负环即无解，没有对n点更新即随便放（即没有约束条件），其他情况输出最短路就行了。
>* 链式向前星存图。

# 代码
```c++
//#include<bits/stdc++.h>
#include<iostream>
#include<vector>
#include<queue>
#include<stdio.h>
#include<string.h>
#define CRL(a,x) memset(a,x,sizeof(a))
#define INF 0xfffffff
using namespace std;
const int N=1e3+5;
struct node{int To,S,Next;};
vector<node> Edge;
int Begin[N],Vis[N],Dis[N],Inque[N],n;

void add(int x,int y,int z){
    node tem=(node){y,z,Begin[x]};
    Begin[x]=Edge.size();
    Edge.push_back(tem);
}

void Spfa(int s){
    queue <int> Q; Inque[s]=1;Dis[s]=0;
    Q.push(s);
    while(!Q.empty()){
        int T=Q.front();Q.pop();Inque[T]=0;
        for(int i=Begin[T];i!=-1;i=Edge[i].Next){
             if(Dis[Edge[i].To]>Dis[T]+Edge[i].S){
                Dis[Edge[i].To]=Dis[T]+Edge[i].S;
                if(!Inque[Edge[i].To]){
                    Inque[Edge[i].To]=1;
                    Q.push(Edge[i].To);
                    if(++Vis[Edge[i].To]>n){
                        puts("-1");
                        return ;
                    } 
                }
            }
        }
    }
    if(Dis[n]!=INF) printf("%d\n",Dis[n]);
    else puts("-2");
}

int main()
{
    int ML,MD,a,b,d;
    while(~scanf("%d%d%d",&n,&ML,&MD)){
        Edge.clear();CRL(Begin,-1);CRL(Vis,0);CRL(Inque,0);
        for(int i=0;i<=n;i++) Dis[i]=INF;
        while(ML--){
            scanf("%d%d%d",&a,&b,&d);
            add(a,b,d);
        }
        while(MD--){
            scanf("%d%d%d",&a,&b,&d);
            add(b,a,-d);
        }
    
        Spfa(1);
    }
    return 0;
}
```
