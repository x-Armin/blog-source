---
title: 一个人的旅行(Dijkstra算法)
date: 2018-02-18 18:13:01
tags: [图论,最短路]
categories: 题解
mathjax: true
copyright: true
---
# 描述
传送门：[hdu-2066](http://acm.hdu.edu.cn/showproblem.php?pid=2066)

>&emsp;虽然草儿是个路痴（就是在杭电待了一年多，居然还会在校园里迷路的人，汗~),但是草儿仍然很喜欢旅行，因为在旅途中 会遇见很多人（白马王子，^0^），很多事，还能丰富自己的阅历，还可以看美丽的风景……草儿想去很多地方，她想要去东京铁塔看夜景，去威尼斯看电影，去阳明山上看海芋，去纽约纯粹看雪景，去巴黎喝咖啡写信，去北京探望孟姜女……眼看寒假就快到了，这么一大段时间，可不能浪费啊，一定要给自己好好的放个假，可是也不能荒废了训练啊，所以草儿决定在要在最短的时间去一个自己想去的地方！因为草儿的家在一个小镇上，没有火车经过，所以她只能去邻近的城市坐火车（好可怜啊~）。

<!--more-->
## Input
>输入数据有多组，每组的第一行是三个整数T，S和D，表示有T条路，和草儿家相邻的城市的有S个，草儿想去的地方有D个；
>接着有T行，每行有三个整数a，b，time,表示a,b城市之间的车程是time小时；(1=<(a,b)<=1000;a,b 之间可能有多条路)
>接着的第T+1行有S个数，表示和草儿家相连的城市；
>接着的第T+2行有D个数，表示草儿想去地方。

## Output
>输出草儿能去某个喜欢的城市的最短时间。


## Examples
* intput
```
6 2 3
1 3 5
1 4 7
2 8 12
3 8 4
4 9 12
9 10 2
1 2
8 9 10
```
* output
```
9
```

# 思路
>* 一读完题就开始敲Floyd，然后没有意外地T了。
>* ~~经过大佬的一番点拨，~~我们把草儿家看成一个节点0，邻近的城市和草儿家的距离为0，这样就转换成了最单纯的单源最短路问题。

# 代码
```c++
#include<iostream>
#include<algorithm>
#include<string.h>
#include<string>
#include<stdio.h>
#include<stdlib.h>
#include<math.h>
using namespace std;
#define CRL(a) memset(a,0,sizeof(a))
typedef unsigned long long LL;
typedef  long long ll;
const int mod =1e9+7;
#define MAX 99999
bool visit[1005];
int Distance[1005],map[1005][1005],m,n,MaxNode;//	Distance:源点到点i的距离	MaxNode:最大的节点

void Dijkstar(int v0)	//v0为源点
{
    CRL(visit);
    visit[v1]=1;
    for(int i=1; i<=MaxNode; i++)
        Distance[i]=map[v1][i];

    int k=1,t=1,v,w,Min;

    // 开始主循环，每次求得V0到某个V顶点的最短路径
    for( v=1; v<=MaxNode; v++ )
    {
        Min=MAX;
        for(w=1; w<=MaxNode; w++ )
        {
            if(!visit[w]&&Distance[w]<Min )
            {
                k=w;
                Min=Distance[w];
            }
        }
        visit[k]=1;	// 将目前找到的最近的顶点置1

        // 修正当前最短路径及距离
        for(w=0; w<=MaxNode; w++)
        {
            // 如果经过v顶点的路径比现在这条路径的长度短就修正
            if( !visit[w] && (Min+map[k][w]<Distance[w]) )
                Distance[w]=Min+map[k][w];	// 修改当前路径长度
        }
    }
}

void create()		//建图
{
    int x,y,way;
    memset(map,1,sizeof(map));	
    //把map数组全部置为无穷大，准确来说现在map中每个元素在内存中为00000001000000010000000100000001 
    for(int i=1; i<=m; i++)
    {
        scanf("%d%d%d",&x,&y,&way);
        MaxNode=MaxNode>x?MaxNode:x;
        MaxNode=MaxNode>y?MaxNode:y;
        if(way<map[x][y])
            map[x][y]=map[y][x]=way;
    }
    for(int i=0; i<n; i++)
    {
        cin>>x;
        map[0][x]=map[x][0]=0;
    }
    return;
}

int main()
{
    int T,ans=99999,want;
    while(cin>>m>>n>>T)
    {
        ans=99999;
        create();
        Dijkstra(0);
        for(int i=0; i<T; i++)
        {
            cin>>want;
            ans=min(ans,Distance[want]);
        }
        cout<<ans<<endl;
    }
    return 0;
}

```