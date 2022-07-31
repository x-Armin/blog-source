---
title: One Person Game(拓展欧几里德算法)
mathjax: true
copyright: true
date: 2018-04-09 18:47:50
tags: [数论,拓欧]
categories: 题解
---
# 描述
传送门：[zoj-3593](http://acm.zju.edu.cn/onlinejudge/showProblem.do?problemId=4677)

>&emsp;There is an interesting and simple one person game. Suppose there is a number axis under your feet. You are at point $A$ at first and your aim is point $B$. There are 6 kinds of operations you can perform in one step. That is to go left or right by $a,b$ and $c$, here $c$ always equals to $a+b$. 

>You must arrive B as soon as possible. Please calculate the minimum number of steps. 

<!--more-->
## Input
>There are multiple test cases. The first line of input is an integer $T(0 < T ≤ 1000) $indicates the number of test cases. Then T test cases follow. Each test case is represented by a line containing four integers 4 integers$ A, B, a\ $and $b$, separated by spaces. ($-2^{31} ≤ A, B < 2^{31}, 0 < a, b < 2^{31}$) 

## Output
>&emsp;For each test case, output the minimum number of steps. If it's impossible to reach point B, output **"-1"** instead. 

## Examples
* intput
```
2
0 1 1 2
0 1 2 4
```
* output
```
1
-1
```

# 思路
>* 一开始以为是道bfs，敲到一半看到数据范围就发现天真了。
>* 题目其实就是求$|C_1a+C_2b+C_3c+C_4(-a)+C_5(-b)+C_6(-c)|=|A-B|$，化简之后就变成了$|C_1a+C_2b|=|A-B|$，还是不满足拓展欧几里德的形式。要变形一下就变成了
$$\frac{C_1\cdot gcd(a,b)}{|A-B|}\cdot a+\frac{C_2\cdot gcd(a,b)}{|A-B|}\cdot b=gcd(a,b)$$
>* 我们还知道拓展欧几里德的通解是$ x=x_0+(b/gcd)\cdot t$，$y=y_0-(a/gcd)\cdot t$，($y_0$和$x_0$为一组特解)。这时候我们带出$C_1,C_2$，画出$C_1,C_2$关于$t$的两条直线，显然当$x,y$异号的时候，$ans=|x+y|$，当$x,y$同号的时候，$ans=max(|a|,|y|)$,结合图像，显然交点为最优解。但交点对应的$t$不一定是整数点，所以我们在对$t$取整后还要比较$t$取$t+1$时的情况。
>* 拓展欧几里德详见：[数论笔记本](http://x-armin.com/%E6%95%B0%E8%AE%BA%E7%AC%94%E8%AE%B0%E6%9C%AC/)

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
#define MAX 0xffffffffff        //MAX取足够大
typedef unsigned long long LL;
typedef  long long ll;

ll x,y;
ll exgcd(int a,int b)		//拓展欧几里得
{
    if(b==0)
    {
        x=1;
        y=0;
        return a;
    }
    int ans=exgcd(b,a%b);
    int t=x;
    x=y;
    y=t-a/b*y;
    return ans;
}

int main()
{
    ll A,B,a,b,n,ans1,L,ans2;
    cin>>n;
    while(n--)
    {
        ans1=MAX;		//注意MAX要取足够大，要取超过 int的最大值（因为这个wa了无数发）
        cin>>A>>B>>a>>b;

        L=abs(A-B);
        ll gcd=exgcd(a,b);

        if(x>y)   //为下面x,y的表达式做的处理
        {
            swap(x, y);
            swap(a, b);
        }

        x=x*L/gcd;
        y=y*L/gcd;

        ll t=(y-x)*gcd/(a+b);	//t等于交点

        a/=gcd;
        b/=gcd;

        for(ll i=t; i<=t+1; i++)	//在t和t+1中找最优解
        {
            if((x+b*i)*(y-a*i)>0)
                ans2=max(x+b*i,y-a*i);
            else
                ans2=abs(x+b*i)+abs(y-a*i);

            ans1=min(ans1,ans2);
        }

        if(L%gcd)			//无解
            cout<<-1<<endl;
        else
            cout<<ans1<<endl;
    }
    return 0;
}
```
