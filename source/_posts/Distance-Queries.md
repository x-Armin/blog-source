---
title: Distance Queries（带权LCA）
mathjax: true
copyright: true
date: 2018-08-11 14:31:48
tags: [LCA,Tarjan,图论]
categories: 题解
---
# 描述
传送门：[poj-1986](http://poj.org/problem?id=1986)

>&emsp;Farmer John's cows refused to run in his marathon since he chose a path much too long for their leisurely lifestyle. He therefore wants to find a path of a more reasonable length. The input to this problem consists of the same input as in **"Navigation Nightmare"** ,followed by a line containing a single integer K, followed by K "distance queries". Each distance query is a line of input containing two integers, giving the numbers of two farms between which FJ is interested in computing distance (measured in the length of the roads along the path between the two farms). Please answer FJ's distance queries as quickly as possible! 

<!--more-->
## Input
>* Lines 1..1+M: Same format as **"Navigation Nightmare"** 

>* Lines 2+M: A single integer, K. 1 <= K <= 10,000 

>* Lines 3+M..2+M+K: Each line corresponds to a distance query and contains the indices of two farms. 

## Output
>* Lines 1..K: For each distance query, output on a single line an integer giving the appropriate distance. 

## Examples
* intput
```c++
7 6
1 6 13 E
6 3 9 E
3 5 7 S
4 1 3 N
2 4 20 W
4 7 2 S
3
1 6
1 4
2 6
```
* output
```c++
13
3
36
```

# 大致题意
>* 给出m条边和k个询问，按顺序求出每次询问的两个点的最短距离。
>* 这道题输入巨坑！它给边的规则描述在另一道题上，就是题目里的 **"[Navigation Nightmare](http://poj.org/problem?id=1984)"**。然后我建单向边从root跑wa了无数遍。

# 思路
>* 最短路$O(N^2 \log N))$是会T的。
>* LCA模板题，维护一个$Dis$数组，$Dis[i]$表示i节点到根节点的距离，查询的时候，u和v的最短距离就是$Dis[u]+Dis[v]-2*Dis[LCA(u,v)]$

# 代码
```c++
#include <iostream>
#include <string.h>
#include <vector>
#include <stdio.h>
using namespace std;
#define rep(i,a,n) for(int i=a;i<n;i++)
#define repd(i,a,n) for(int i=n-1;i>=a;i--)
#define CRL(a,x) memset(a,x,sizeof(a))
#define Tp(a,b) make_pair(a,b)
const int N = 4e4 + 5;

int Fa[N], Dis[N], Begin[N],ans[10005]; vector < pair<int,int> > Qus[N]; bool Vis[N];
struct node { int To, S, Next; };
vector<node> Edge;

void Init() {
	Edge.clear();
	rep(i, 0, N) { Fa[i] = i; Qus[i].clear(); }
	CRL(Dis, 0); CRL(Begin, -1); CRL(Vis, false);
}

void add(int a, int b, int s) {
	Edge.push_back(node{ b,s,Begin[a]});
	Begin[a] = Edge.size() - 1;
}

int Find(int x) { return x == Fa[x] ? x : Fa[x] = Find(Fa[x]); }

void Tarjan(int u,int fa) {
	for (int i = Begin[u]; ~i; i = Edge[i].Next) {
		int v = Edge[i].To;
		if(v==fa) continue;                 //双向边，不往回走
		Dis[v] = Edge[i].S + Dis[u];
		Tarjan(v,u);
		Fa[v] = u;
	}
    Vis[u] = true;
	rep(i, 0, Qus[u].size())
		if (Vis[Qus[u][i].first])
			ans[Qus[u][i].second] = Dis[u] + Dis[Qus[u][i].first] - Dis[Find(Qus[u][i].first)]*2;
}

int main()
{
	int n, m, a, b, s; char x;
	while(~scanf("%d%d",&n,&m)){
        Init();
        rep(i, 0, m) {
            scanf("%d%d%d %c",&a,&b,&s,&x);
            Fa[b] = a;
            add(a, b, s);
            add(b, a, s);
        }

        scanf("%d",&m);
        rep(i, 0, m) {
            scanf("%d%d",&a,&b);
            Qus[a].push_back(Tp(b,i));      //pair.second记录询问顺序
            Qus[b].push_back(Tp(a,i));
        }

        int root = Find(a);
        rep(i, 0, n + 1) Fa[i] = i;
        Tarjan(root,0);

        rep(i,0,m) printf("%d\n",ans[i]);
	}
	return 0;
}

```
