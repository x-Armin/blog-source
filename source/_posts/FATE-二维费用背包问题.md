---
title: FATE(二维费用背包问题)
mathjax: true
copyright: true
date: 2018-03-02 15:57:23
tags: [背包问题,动态规划]
categories: 题解
---
# 描述
传送门：[hdu-2159](http://acm.hdu.edu.cn/showproblem.php?pid=2159)

>&emsp;最近xhd正在玩一款叫做FATE的游戏，为了得到极品装备，xhd在不停的杀怪做任务。久而久之xhd开始对杀怪产生的厌恶感，但又不得不通过杀怪来升完这最后一级。现在的问题是，xhd升掉最后一级还需n的经验值，xhd还留有m的忍耐度，每杀一个怪xhd会得到相应的经验，并减掉相应的忍耐度。当忍耐度降到0或者0以下时，xhd就不会玩这游戏。xhd还说了他最多只杀s只怪。请问他能升掉这最后一级吗？

<!--more-->
## Input
>输入数据有多组，对于每组数据第一行输入n，m，k，s(0 < n,m,k,s < 100)四个正整数。分别表示还需的经验值，保留的忍耐度，怪的种数和最多的杀怪数。接下来输入k行数据。每行数据输入两个正整数a，b(0 < a,b < 20)；分别表示杀掉一只这种怪xhd会得到的经验值和会减掉的忍耐度。(每种怪都有无数个)


## Output
>输出升完这级还能保留的最大忍耐度，如果无法升完这级输出-1。

## Examples
* intput
```
10 10 1 10
1 1
10 10 1 9
1 1
9 10 2 10
1 1
2 2
```
* output
```
0
-1
1
```

# 思路
>* 有两个费用，一个是忍耐度，另一个是杀怪数。
>* 注意这里对经验的理解，它不是第三个费用。
>* 然后用完全背包，最后判断一下最优解即可。

# 代码
```c++
//#include<bits/stdc++.h>
#include<iostream>
#include<algorithm>
#include<string.h>
#include<string>
#include<stdio.h>
#include<stdlib.h>
#include<math.h>
using namespace std;
#define CRL(a) memset(a,0,sizeof(a))
#define MAX 0xfffffff
typedef unsigned long long LL;
typedef  long long ll;
const double Pi = acos(-1);
const double e = 2.718281828459;
const int mod =1e9+7;

int dp[105][105],n,m,k,s,cost[105],valum[105];  //dp[i][j]:杀i,耐力j时的经验

int main()
{
    while(cin>>n>>m>>k>>s)
    {
        for(int i=0; i<k; i++)
            cin>>valum[i]>>cost[i];

        CRL(dp);

        for(int i=0; i<k; i++)
            for(int p=1; p<=s; p++)				 //杀怪
                for(int j=cost[i]; j<=m; j++)		//忍耐
                    dp[p][j]=max(dp[p][j],dp[p-1][j-cost[i]]+valum[i]);

        if(dp[s][m]<n)				//如果消耗完耐力都不能升级
            cout<<-1<<endl;
        else
            for(int i=0; i<=m; i++)	//找能升级，耐力消耗最小的
                if(dp[s][i]>=n)
                {
                    cout<<m-i<<endl;
                    break;
                }
    }
    return 0;
}

```