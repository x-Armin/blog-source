---
title: Transport Ship(多重背包)
mathjax: true
copyright: true
date: 2018-09-17 00:55:27
tags: [背包问题,二进制,dp]
categories: 题解
---
# 描述
传送门：[ACM-ICPC 2018 焦作赛区网络预赛 K](https://nanti.jisuanke.com/t/31720)

>&emsp;There are $N$ different kinds of transport ships on the port. The $i^{th}$kind of ship can carry the weight of $V[i]$ and the number of the $i^{th}$ kind of ship is $2^{C[i]} - 1$. How many different schemes there are if you want to use these ships to transport cargo with a total weight of $S$?

> It is required that each ship must be full-filled. Two schemes are considered to be the same if they use the same kinds of ships and the same number for each kind.

<!--more-->
## Input
> The first line contains an integer $T(1 \le T \le 20)$, which is the number of test cases.

> For each test case:

> The first line contains two integers: $N(1 \le N \le 20)$, $Q(1 \le Q \le 10000)$, representing the number of kinds of ships and the number of queries.

> For the next $N$ lines, each line contains two integers: $V[i]\(1 \le V[i] \le 20\)$ , $C[i]\(1 \le C[i] \le 20\)$, representing the weight the $i^{th}$ kind of ship can carry, and the number of the $i^{th}$ kind of ship is $2^{C[i]} - 1$.

> For the next $Q$ lines, each line contains a single integer: $S(1 \le S \le 10000)$, representing the queried  weight.

## Output
> For each query, output one line containing a single integer which represents the number of schemes for arranging ships. Since the answer may be very large, output the answer modulo $1000000007$.

## Examples
* intput
```c++
1
1 2
2 1
1
2
```
* output
```c++
0
1
```

# 思路
>* 完全背包二进制优化，因为数量都是$2^n-1$,也就是说二进制的所有位数都是1，就可以保证不会有相同种类相同数量的。
>* <font size=5 >$$dp[i]=(dp[i]+dp[i-cost]) \% mod$$<front>;

# 代码
```c++
#include<bits/stdc++.h>
using namespace std;
#define rep(i,a,n) for(int i=a;i<n;i++)
#define repd(i,a,n) for(int i=n-1;i>=a;i--)
#define CRL(a,x) memset(a,x,sizeof(a))
#define MAX 0xfffffff
typedef unsigned long long LL;
typedef  long long ll;
const double Pi = acos(-1);
const double e = exp(1.0);
const int mod =1e9+7;

int dp[10005],n,V,Cont[500005],Cost[500005];

void ZeroOnePack(int cost,int cont) {
    for(int i=V; i>=cost; i--)
        if(dp[i-cost]>0&&i>=cost) {
            if(dp[i]==-1) dp[i]++;
            dp[i]=(dp[i]+dp[i-cost])%mod;
        }
}

void CompletePack(int cost,int valum) {
    for(int i=cost; i<=V; i++)
        if(dp[i-cost]>0&&i>=cost) {
            if(dp[i]==-1) dp[i]++;
            dp[i]=(dp[i]+dp[i-cost])%mod;
        }
}

void MultiplePack(int cost,int amount) {
    if(cost*amount>=V)
        CompletePack(cost,amount);
    else {
        int k=1;
        while(k<amount) {
            ZeroOnePack(cost*k,k);
            amount-=k;
            k<<=1;
        }
        ZeroOnePack(cost*amount,amount);
    }
}

int main() {
    ll ans=0;

    int T,que[10005],nq,q;
    scanf("%d",&T);
    while(T--) {
        CRL(dp,-1);
        dp[0]=1;

        scanf("%d%d",&n,&nq);
        rep(i,0,n) {
            scanf("%d%d",&Cost[i],&Cont[i]);
            Cont[i]=(1<<Cont[i])-1;
        }

        rep(i,0,nq) {
            scanf("%d",&que[i]);
            V=max(que[i],V);
        }

        rep(i,0,n) MultiplePack(Cost[i],Cont[i]);、
        rep(i,0,nq) {
            if(dp[que[i]]==-1) puts("0");
            else
                printf("%d\n",dp[que[i]]);
        }
    }

    return 0;
}


```
