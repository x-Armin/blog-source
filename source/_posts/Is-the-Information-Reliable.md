---
title: Is the Information Reliable?（差分约束+链式向前星）
mathjax: true
copyright: true
date: 2018-06-25 02:11:13
tags: [差分约束,图论,最短路,spfa]
categories: 题解
---
# 描述
传送门：[poj-2983](http://poj.org/problem?id=2983)

>&emsp;The galaxy war between the Empire Draco and the Commonwealth of Zibu broke out 3 years ago. Draco established a line of defense called Grot. Grot is a straight line with N defense stations. Because of the cooperation of the stations, Zibu’s Marine Glory cannot march any further but stay outside the line.

> A mystery Information Group X benefits form selling information to both sides of the war. Today you the administrator of Zibu’s Intelligence Department got a piece of information about Grot’s defense stations’ arrangement from Information Group X. Your task is to determine whether the information is reliable.

> The information consists of M tips. Each tip is either precise or vague.

> Precise tip is in the form of P A B X, means defense station A is X light-years north of defense station B.

> Vague tip is in the form of V A B, means defense station A is in the north of defense station B, at least 1 light-year, but the precise distance is unknown.

<!--more-->
## Input
> There are several test cases in the input. Each test case starts with two integers $N (0 < N ≤ 1000)$ and $M (1 ≤ M ≤ 100000)$.The next $M$ line each describe a tip, either in precise form or vague form.

## Output
> Output one line for each test case in the input. Output “**Reliable**” if It is possible to arrange $N$ defense stations satisfying all the $M$ tips, otherwise output “**Unreliable**”.

## Examples
* intput
```
3 4
P 1 2 1
P 2 3 1
V 1 3
P 1 3 1
5 5
V 1 2
V 2 3
V 3 4
V 4 5
V 3 5
```
* output
```
Unreliable
Reliable
```

# 思路
>* $V\ A\ B$ 的情况很容易想到$Dis[A]-Dis[B]>=1 \Longrightarrow Dis[B]-Dis[A]<=-1 $ 这符合了差分约束的形式，但重点是如何处理P A B X。
>* $P\ A\ B\ X$的时候，将$Dis[A]-Dis[B]=x$写成
$\Longrightarrow Dis[A]-Dis[B]<=x,Dis[A]-Dis[B]>=x $
$\Longrightarrow Dis[A]-Dis[B]<=x,Dis[B]-Dis[A]<=-x $,
这样一张图就构造出来了。注意理清楚这个关系，不然太容易写错。
>* 最后跑一个SPFA看看有没有负环，有就消息不可信。
>* 链式向前星存图。
>* 在关闭流同步的情况下，cin输入比scanf输入慢了六倍多。

# 代码
```c++
//#include<bits/stdc++.h>
#include<iostream>
#include<vector>
#include<queue>
#include<stdio.h>
#include<string.h>
#define CRL(a,x) memset(a,x,sizeof(a))
using namespace std;
const int N=1e5+5;

int Begin[N],Dis[N],n;bool Inque[N];
struct node{int To,S,Next;};    //链式向前星节点
vector <node> Edge;

void add(int x,int y,int z){    
    node tem=(node){y,z,Begin[x]};
    Begin[x]=Edge.size();
    Edge.push_back(tem);
}

/*  BFS版  */
bool Spfa(int s){
    int Vis[N]={1};
    queue<int> Q;Q.push(s);
    while(!Q.empty()){
        int tem=Q.front();Q.pop();Inque[tem]=0;     //队首元素进行松弛
        for(int i=Begin[tem];i!=-1;i=Edge[i].Next){
            if(Dis[Edge[i].To]>Dis[tem]+Edge[i].S){
                Dis[Edge[i].To]=Dis[tem]+Edge[i].S;
                if(!Inque[Edge[i].To]){
                    Inque[Edge[i].To]=1;
                    Q.push(Edge[i].To);
                    if(++Vis[Edge[i].To]>n) return true;        //一个节点入队n次就证明有负环。
                }
            }
        }
    }
    return false;
}

int main()
{
    int m,a,b,s;char p;
    while(~scanf("%d%d",&n,&m)){
        Edge.clear();
        CRL(Begin,-1);CRL(Dis,0xf);CRL(Inque,0);

        while(m--){
            getchar(); p=getchar();
            if(p=='P'){
                scanf("%d%d%d",&a,&b,&s);
                add(a,b,-s);        //一开始这里写成了s，wa++;
                add(b,a,s);
            }else{
                scanf("%d%d",&a,&b);
                add(a,b,-1);        //一开始这里写成了1，再wa++;
            }
        }
        for(int i=1;i<=n;i++) add(0,i,0); Dis[0]=0;     //加入超级源点
        puts(Spfa(0)? "Unreliable":"Reliable");
    }
    return 0;
}

/*  //DFS版
bool Spfa(int s){
    for(int i=Begin[s];i!=-1;i=Edge[i].Next){
        if(Dis[Edge[i].To]>Dis[s]+Edge[i].S){
            Dis[Edge[i].To]=Dis[s]+Edge[i].S;
            if(Inque[Edge[i].To]) return true;
            Inque[Edge[i].To]=true;
            if(Spfa(Edge[i].To)) return true;
            Inque[Edge[i].To]=false;
        }
    }
    return false;
}*/

```
