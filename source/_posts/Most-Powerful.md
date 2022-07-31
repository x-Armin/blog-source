---
title: Most Powerful（状压dp入门）
mathjax: true
copyright: true
date: 2018-10-13 20:40:42
tags: [状压dp,dp,动态规划,状压]
categories: 题解
---
# 描述
传送门：[ZOJ-3471](http://acm.zju.edu.cn/onlinejudge/showProblem.do?problemCode=3471)

>&emsp;Recently, researchers on Mars have discovered N powerful atoms. All of them are different. These atoms have some properties. When two of these atoms collide, one of them disappears and a lot of power is produced. Researchers know the way every two atoms perform when collided and the power every two atoms can produce.

> You are to write a program to make it most powerful, which means that the sum of power produced during all the collides is maximal.

<!--more-->
## Input
> There are multiple cases. The first line of each case has an integer $N (2 <= N <= 10)$, which means there are N atoms: $A_1, A_2, ... , A_N$. Then $N$ lines follow. There are $N$ integers in each line. The $j^{th}$ integer on the $i^{th}$ line is the power produced when $A_i$ and $A_j$ collide with $A_j$ gone. All integers are positive and not larger than 10000.

The last case is followed by a 0 in one line.

There will be no more than 500 cases including no more than 50 large cases that $N$ is 10.

## Output
> Output the maximal power these $N$ atoms can produce in a line for each case.

## Examples
* intput
```c++
2
0 4
1 0
3
0 20 1
12 0 1
1 10 0
0
```
* output
```c++
4
22
```

# 思路
>* 状态dp入门题，用0来表示未泯灭，1表示已经泯灭，这样就能用一个n位的整数来记录所有的状态。
>* 对于状态i，假设第j位和第k位都是0，那么我们就可以通过dp[i]推出 dp[i&1<<k]的状态。
>* 状态转移方程：
$$
dp[i|1<<k]=max(dp[i]+a[j][k],dp[i|1<<k])
$$

# 代码
```c++
#include <bits/stdc++.h>
using namespace std;
#define rep(i,a,n) for(int i=a;i<n;i++)
#define repd(i,a,n) for(int i=n-1;i>=a;i--)
#define CRL(a,x) memset(a,x,sizeof(a))

int book[12][12],n,dp[1<<12],ans=0;

int main()
{
    while(scanf("%d",&n)&&n){
        CRL(dp,0);ans=0;
        rep(i,0,n)
            rep(j,0,n)
                scanf("%d",&book[i][j]);

        rep(i,0,1<<n)       //所有状态
            rep(j,0,n)      //泯灭的原子
                if(!(i&1<<j))//泯灭的原子要是0
                    rep(k,0,n)//被碰撞的原子
                        if(j!=k&&!(i&1<<k)) //被碰撞的原子要是0且不是泯灭的原子
                            dp[i|1<<k]=max(dp[i]+book[j][k],dp[i|1<<k]);

        rep(i,0,1<<n) ans=max(dp[i],ans);
        printf("%d\n",ans);
    }
    return 0;
}

```
