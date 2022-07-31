---
title: C Looooops(拓展欧几里德)
mathjax: true
copyright: true
date: 2018-04-26 21:21:40
tags: [数论,拓欧]
categories: 题解
---
# 描述
传送门：[poj-2115](http://poj.org/problem?id=2115)

>A Compiler Mystery: We are given a C-language style for loop of type 
for(variable = A; variable != B; variable += C)
    statement;

> Ie, a loop which starts by setting variable to value A and while variable is not equal to B, repeats statement followed by increasing the variable by C. We want to know how many times does the statement get executed for particular values of A, B and C, assuming that all arithmetics is calculated in a k-bit unsigned integer type (with values 0 <= x < $2^k$) modulo $2^k$. 

<!--more-->
## Input
> The input consists of several instances. Each instance is described by a single line with four integers A, B, C, k separated by a single space. The integer k (1 <= k <= 32) is the number of bits of the control variable of the loop and A, B, C (0 <= A, B, C < 2 k) are the parameters of the loop. 

> The input is finished by a line containing four zeros. 

## Output
> The output consists of several lines corresponding to the instances on the input. The i^(th) line contains either the number of executions of the statement in the i^(th) instance (a single integer number) or the word "FOREVER" if the loop does not terminate. 

## Examples
* intput
```
3 3 2 16
3 7 2 16
7 3 2 16
3 4 2 16
0 0 0 0
```
* output
```
0
2
32766
FOREVER
```

# 思路
>* $k=2^k$,即求$a+t \cdot c-k \cdot m=b$，求最小正数$t$，拓欧基本操作。

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
#define INF 0xfffffff
typedef unsigned long long LL;
typedef  long long ll; 
const double Pi = acos(-1);
const double e = exp(1.0);
const int mod =1e9+7; 

ll exgcd(ll a,ll b,ll &x,ll &y)
{
    if(b==0) return x=1,y=0,a;
    ll tmp=exgcd(b,a%b,y,x);
    y-=a/b*x;
    return tmp;
}

int main()
{
    ll a,b,c,k,x,y,d,t;
    while(~scanf("%lld%lld%lld%lld",&a,&b,&c,&k)&&(a||b||c||k))
    {
        k=(1LL)<<k;         //注意1要转成long long的1,否则k=32时要爆精度。
        t=b-a;
        ll gcd=exgcd(c,k,x,y);
        if((b-a)%gcd) printf("FOREVER\n");
        else{
            x*=(b-a)/gcd; 
            k /= gcd;  
            x = (x%k + k) % k;  
            printf("%lld\n",x);
        }
    }
    return 0;
}
```
