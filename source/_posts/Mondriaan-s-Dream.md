---
title: Mondriaan's Dream（dfs+状压dp）
mathjax: true
copyright: true
date: 2018-11-08 11:00:49
tags: [状压dp,dp,动态规划,状压,dfs]
categories: 题解
---
# 描述
传送门：[poj-2411](http://poj.org/problem?id=2411)

>&emsp;Squares and rectangles fascinated the famous Dutch painter Piet Mondriaan. One night, after producing the drawings in his 'toilet series' (where he had to use his toilet paper to draw on, for all of his paper was filled with squares and rectangles), he dreamt of filling a large rectangle with small rectangles of width 2 and height 1 in varying ways. 
![](http://poj.org/images/2411_1.jpg)&emsp;Expert as he was in this material, he saw at a glance that he'll need a computer to calculate the number of ways to fill the large rectangle whose dimensions were integer values, as well. Help him, so that his dream won't turn into a nightmare!

<!--more-->
## Input
> The input contains several test cases. Each test case is made up of two integer numbers: the height h and the width w of the large rectangle. Input is terminated by $h=w=0$. Otherwise, $1<=h,w<=11$.

## Output
> For each test case, output the number of different ways the given rectangle can be filled with small rectangles of size 2 times 1. Assume the given large rectangle is oriented, i.e. count symmetrical tilings multiple times.

## Examples
* intput
```c++
1 2
1 3
1 4
2 2
2 3
2 4
2 11
4 11
0 0
```
* output
```c++
1
0
1
2
3
5
144
51205
```

# 思路
>* dp[i][j]：i行在j状态的方案数
>* 对于每一行，dfs所有能放的情况。
>* 状态转移方程(i:行 state:i行的状态 nex:i+1行的状态)：
$$dp[i+1][nex]+=dp[i][state]$$

# 代码
```c++
/*
Problem: 2411		User: Armin
Memory: 1140K		Time: 16MS
Language: G++		Result: Accepted
*/
#include<stdio.h>
#include<algorithm>
#include<string.h>
using namespace std;
#define rep(i,a,n) for(int i=a;i<n;i++)
#define repd(i,a,n) for(int i=n-1;i>=a;i--)
#define CRL(a,x) memset(a,x,sizeof(a))
typedef long long ll;
const int N=13;
int n,m; ll dp[N][1<<N];

void dfs(int i,int j,int state,int nex){    //i:当前行  j:当前列  state:i行的状态  nex:i+1行的状态
    if(j==n){           //如果到达边界，直接加
        dp[i+1][nex]+=dp[i][state]; 
        return;
    }
    if(1<<j&state)      //如果j位置放了东西
        dfs(i,j+1,state,nex);

    if(!(1<<j&state))   //如果j位置是空，竖着放一个
        dfs(i,j+1,state,1<<j|nex);//由于竖着放了一个，所以下一行的j位置就被占用了。

    if(j+1<n&&!(1<<j&state)&&!((1<<j+1&state))) //如果j位置和j+1位置都为空，横放一个
        dfs(i,j+2,state,nex);
}

int main()
{
    while(scanf("%d%d",&n,&m)&&(n||m)){
        CRL(dp,0);
        dp[0][0]=1;
        if(n>m) swap(n,m);

        rep(i,0,m)
            rep(j,0,1<<n)
                if(dp[i][j])//剪枝：dp[i][j]不为0才能更新否则更新没意义
                    dfs(i,0,j,0);
        printf("%lld\n",dp[m][0]);
    }
    return 0;
}

```
