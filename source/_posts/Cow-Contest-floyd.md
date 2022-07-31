---
title: Cow Contest(floyd))
date: 2018-02-14 12:50:55
tags: [图论]
categories: 题解
mathjax: true
copyright: true
---
# 描述
传送门：[poj-3660](http://poj.org/problem?id=3660)

>N (1 ≤ N ≤ 100) cows, conveniently numbered 1..N, are participating in a programming contest. As we all know, some cows code better than others. Each cow has a certain constant skill rating that is unique among the competitors.

>The contest is conducted in several head-to-head rounds, each between two cows. If cow A has a greater skill level than cow B (1 ≤ A ≤ N; 1 ≤ B ≤ N; A ≠ B), then cow A will always beat cow B.

>Farmer John is trying to rank the cows by skill level. Given a list the results of M (1 ≤ M ≤ 4,500) two-cow rounds, determine the number of cows whose ranks can be precisely determined from the results. It is guaranteed that the results of the rounds will not be contradictory.

<!--more-->
## Input
>* Line 1: Two space-separated integers: N and M
>* Lines 2..M+1: Each line contains two space-separated integers that describe the competitors and results (the first integer, A, is the winner) of a single round of competition: A and B.

## Output
>Line 1: A single integer representing the number of cows whose ranks can be determined.

## Examples
* intput
```
5 5
4 3
4 2
3 2
1 2
2 5
```
* output
```
2
```

# 思路
>* 把比赛看成是一个有向图，问题其实就转化成求了求有多少个顶点和其他所有顶点都连通。
>* 形象地说就是当一头牛胜负加起来为n-1时，也就是和其他的牛都比赛过(包括间接的)就能确定它的名次。

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
#define MAX 0xfffffff
typedef unsigned long long LL;
typedef  long long ll;
const int mod =1e9+7;

int map[105][105],win[105],defeat[105];

int main()
{
    int n,w,a,b,ans;
    while(cin >> n >> w)
    {
        ans=0;
        CRL(map);
        CRL(win);
        CRL(defeat);
        while(w--)
        {
            cin>>a>>b;
            if(!map[a][b])
            {
                map[a][b]=1;
                win[a]++;
                defeat[b]++;
            }

        }

        for(int k=1; k<=n; k++)
            for(int j=1; j<=n; j++)
                for(int i=1; i<=n; i++)
                    if(map[j][k]&&map[k][i]&&!map[j][i])
                    {
                        map[j][i]=1;
                        win[j]++;
                        defeat[i]++;
                    }

        for(int i=1; i<=n; i++)
            if(win[i]+defeat[i]+1==n)
                ans++;

        cout<<ans<<endl;
    }
    return 0;
}

```
