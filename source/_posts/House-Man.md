---
title: House Man（差分约束+链式向前星）
mathjax: true
copyright: true
date: 2018-06-29 16:57:38
tags: [差分约束,图论,最短路,SPFA]
categories: 题解
---
# 描述
传送门：[hdu-3440](http://acm.hdu.edu.cn/showproblem.php?pid=3440)

>&emsp;In Fuzhou, there is a crazy super man. He can’t fly, but he could jump from housetop to housetop. Today he plans to use $N$ houses to hone his house hopping skills. He will start at the shortest house and make $N$-1 jumps, with each jump taking him to a taller house than the one he is jumping from. When finished, he will have been on every house exactly once, traversing them in increasing order of height, and ending up on the tallest house. 

<!--more-->

>The man can travel for at most a certain horizontal distance $D$ in a single jump. To make this as much fun as possible, the crazy man want to maximize the distance between the positions of the shortest house and the tallest house. 
The crazy super man have an ability—move houses. So he is going to move the houses subject to the following constraints:
1. All houses are to be moved along a one-dimensional path. 
2. Houses must be moved at integer locations along the path, with no two houses at the same location. 
3. Houses must be arranged so their moved ordering from left to right is the same as their ordering in the input. They must NOT be sorted by height, or reordered in any way. They must be kept in their stated order. 
4. The super man can only jump so far, so every house must be moved close enough to the next taller house. Specifically, they must be no further than $D$ apart on the ground (the difference in their heights doesn't matter). 

>Given $N$ houses, in a specified order, each with a distinct integer height, help the super man figure out the maximum possible distance they can put between the shortest house and the tallest house, and be able to use the houses for training. 

## Input
> In the first line there is an integer T, indicates the number of test cases.$(T\ <=\ 500)$
Each test case begins with a line containing two integers $N\ (1\ ≤ N\ ≤\ 1000)$ and $D\ (1\ ≤\ D\ ≤1000000)$. The next line contains $N$ integer, giving the heights of the $N$ houses, in the order that they should be moved. Within a test case, all heights will be unique. 

## Output
> For each test case , output "Case %d: "first where d is the case number counted from one, then output a single integer representing the maximum distance between the shortest and tallest house, subject to the constraints above, or **-1** if it is impossible to lay out the houses. Do not print any blank lines between answers.

## Examples
* intput
```
3
4 4 
20 30 10 40 
5 6 
20 34 54 10 15 
4 2 
10 20 16 13 
```
* output
```
Case 1: 3
Case 2: 3
Case 3: -1
```

# 思路
>* **高度相近**的两个楼的距离不得超过$D$，也就是$|Dis_i-Dis_{i+1}|<=D$，减的时候保证大减小就能去掉绝对值，这是第一个约束方程。
>* **坐标相近**的两个楼的距离应该>=1.即$Dis_{i+1}-Dis_i>=1$，转化一下就是$Dis_i-Dis_{i+1}<=-1$，这是第二个约束方程，
>* 最大的坑点！！！！Dis的初始值一定要足够大，0xfffffff都过不了，最后取的0x3f3f3f3f。
>* haishitaicaile.

# 代码
```c++
#include<bits/stdc++.h>
using namespace std;
#define CRL(a) memset(a,0,sizeof(a))
#define CRL2(a,x) memset(a,x,sizeof(a))
#define INF 0x3f3f3f3f
const int N=1e3+5;

struct node{int To,S,Next;};
vector <node> Edge;
pair<int,int> Node[N];
int Begin[N],Vis[N],Inque[N],Dis[N],n;

void add(int a,int b,int w){
    node tem=(node){b,w,Begin[a]};
    Begin[a]=Edge.size();
    Edge.push_back(tem);
}

bool spfa(int s){
    for(int i=0;i<=n;i++) Dis[i]=INF; Dis[s]=0;
    CRL(Vis);Vis[s]=1;
    CRL(Inque);Inque[s]=1;
    queue<int> Q;Q.push(s);
    while(!Q.empty()){
        int T=Q.front();Q.pop();Inque[T]=0;
        for(int i=Begin[T];i!=-1;i=Edge[i].Next){
            if(Dis[Edge[i].To]>Dis[T]+Edge[i].S){
                Dis[Edge[i].To]=Dis[T]+Edge[i].S;
                Q.push(Edge[i].To);
                Inque[Edge[i].To]=1;
                if(++Vis[Edge[i].To]>n)return true ;
            }
        }
    }
    return false;
}

int main()
{
    int T,D;
    cin>>T;
    for (int Case=1; Case<=T;Case++){
        Edge.clear();CRL2(Begin,-1);
        scanf("%d%d",&n,&D);
        for(int i=1;i<=n;i++){
            cin>>Node[i].first;
            Node[i].second=i;
        }

        sort(Node+1,Node+1+n);          //对高度排序

        for(int i=1;i<n;i++){
            add(i+1,i,-1);                                  //第二个约束方程
            if(Node[i].second<Node[i+1].second)             //第一个约束方程
                add(Node[i].second,Node[i+1].second,D) ;
            else
                add(Node[i+1].second,Node[i].second,D) ;
        }

        printf("Case %d: ",Case);
        if(spfa(min(Node[1].second,Node[n].second))) printf("-1\n");    //负环无解
        else printf("%d\n",Dis[max(Node[1].second,Node[n].second)]);

    }
    return 0;
}
```
