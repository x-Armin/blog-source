---
title: Corn Fields（状压dp入门）
mathjax: true
copyright: true
date: 2018-10-13 20:40:21
tags: [状压dp,dp,动态规划,状压]
categories: 题解
---
# 描述
传送门：[POJ-3254](http://poj.org/problem?id=3254)

>&emsp;Farmer John has purchased a lush new rectangular pasture composed of $M$ by $N (1 ≤ M ≤ 12; 1 ≤ N ≤ 12)$ square parcels. He wants to grow some yummy corn for the cows on a number of squares. Regrettably, some of the squares are infertile and can't be planted. Canny FJ knows that the cows dislike eating close to each other, so when choosing which squares to plant, he avoids choosing squares that are adjacent; no two chosen squares share an edge. He has not yet made the final choice as to which squares to plant.

> Being a very open-minded man, Farmer John wants to consider all possible options for how to choose the squares for planting. He is so open-minded that he considers choosing no squares as a valid option! Please help Farmer John determine the number of ways he can choose the squares to plant.
<!--more-->
## Input
> Line 1: Two space-separated integers: $M$ and $N$
Lines 2.. $M+1$: Line i+1 describes row i of the pasture with $N$ space-separated integers indicating whether a square is fertile (1 for fertile, 0 for infertile)
## Output
> Line 1: One integer: the number of ways that FJ can choose the squares modulo $100,000,000$。

## Examples
* intput
```c++
2 3
1 1 1
0 1 0
```
* output
```c++
9
```

# 思路
>* 用二进制表示每行的状态，0表示能放，1表示不能放。
>* i&a[j]==i 表示第j行可以放状态i。
>* k&(k<<1)表示 k状态没相邻的1
>* 状态转移方程：
$$
dp[i][j]+=dp[i][j]+dp[i-1][k]
$$


# 代码
```c++
#include<bits/stdc++.h>
using namespace std;
#define rep(i,a,n) for(int i=a;i<n;i++)
#define repd(i,a,n) for(int i=n-1;i>=a;i--)
#define CRL(a,x) memset(a,x,sizeof(a))
const int N=1e6+5;
const int Mod=1e8;

int main() {
    int n,m,a[15]= {0},x,dp[15][5096]= {0},ans=0;
    while(~scanf("%d%d",&n,&m)) {
        CRL(dp,0);CRL(a,0);ans=0;

        rep(i,1,n+1)
            rep(j,0,m) {
                scanf("%d",&x);
                a[i]= (a[i]<<1)|x;
            }

        rep(i,0,1<<m)
            dp[1][i]=!(i&(i<<1))&& (i&a[1])==i;//单独处理第一排

        rep(i,2,n+1)
            rep(j,0,1<<m)
                if((j&a[i])==j&&!(j&(j<<1)))        //如果放的方式合法
                    rep(k,0,1<<m) {
                        if((j&k)||(k&a[i-1])!=k||(k&(k<<1))) continue;
                        dp[i][j]=(dp[i][j]+dp[i-1][k])%Mod;
                }

        rep(i,0,(1<<m)) ans=(ans+dp[n][i])%Mod;
        
        printf("%d\n",ans);
    }

    return 0;
}

```
