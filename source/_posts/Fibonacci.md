---
title: Fibonacci（矩阵快速幂）
mathjax: true
copyright: true
date: 2018-08-21 01:53:07
tags: [数论,矩阵快速幂]
categories: 题解
---
# 描述
传送门：[poj-3070](http://poj.org/problem?id=3070)

>&emsp;In the Fibonacci integer sequence, $F_0 = 0, F_1 = 1$, and $F_n = F_n − 1 + F_n − 2 for n ≥ 2$. For example, the first ten terms of the Fibonacci sequence are:
>
>$$0, 1, 1, 2, 3, 5, 8, 13, 21, 34, …$$
>
>An alternative formula for the Fibonacci sequence is
>$$
\begin{bmatrix}
	F_{n+1} & F_{n}  \\
	F_{n} & F_{n-1}  \\
\end{bmatrix}=
\begin{bmatrix}
	1 & 1  \\
	1 & 0  \\
\end{bmatrix}^n=
\begin{bmatrix}
	1 & 1  \\
	1 & 0  \\
\end{bmatrix}
\begin{bmatrix}
	1 & 1  \\
	1 & 0  \\
\end{bmatrix}...
\begin{bmatrix}
	1 & 1  \\
	1 & 0  \\
\end{bmatrix}
$$
>Given an integer $n$, your goal is to compute the last 4 digits of $F_n$.

<!--more-->
## Input
> The input test file will contain multiple test cases. Each test case consists of a single line containing n (where $0 ≤ n ≤ 1,000,000,000$). The end-of-file is denoted by a single line containing the number **−1**.

## Output
> For each test case, print the last four digits of Fn. If the last four digits of Fn are all zeros, print ‘0’; otherwise, omit any leading zeros (i.e., print Fn mod 10000).

## Examples
* intput
```c++
0
9
999999999
1000000000
-1
```
* output
```c++
0
34
626
6875
```

# 思路
>* 题目直接给出了转移矩阵，显然那个二阶常数矩阵需要用到[矩阵快速幂](http://x-armin.com/%E6%95%B0%E8%AE%BA%E7%AC%94%E8%AE%B0%E6%9C%AC/#%E7%9F%A9%E9%98%B5%E5%BF%AB%E9%80%9F%E5%B9%82)才能求解。
>* 注意单位阵和初始化。 

# 代码
```c++

/*Problem: 3070      Memory: 668K      Time: 0MS      Language: G++      Result:Accepted*/

#include <iostream>
#include <string.h>
using namespace std;
#define rep(i,a,n) for(int i=a;i<n;i++)
#define repd(i,a,n) for(int i=n-1;i>=a;i--)
#define CRL(a,x) memset(a,x,sizeof(a))
const int N=1e3+5;
const int mod=1e4;

struct Mat{
    int data[2][2];
    Mat(){CRL(data,0);} //构造函数

    Mat operator*(const Mat &h){     //重载乘号
        Mat c;
        rep(i,0,2)
            rep(j,0,2)
                rep(k,0,2)
                    c.data[i][j]=(c.data[i][j]+data[i][k]%mod*h.data[k][j]%mod)%mod;
        return c;
    }
}Fn,c;

void Mat_qpow(Mat &Fn,int n){//矩阵快速幂 实际上是c的n次方
    while(n){
        if(n&1) Fn=Fn*c;
        c=c*c;
        n>>=1;
    }
}

int main()
{
    std::ios::sync_with_stdio(false);
    int n;
    while(cin>>n&&~n){
        Fn.data[0][0]=Fn.data[1][1]=1;Fn.data[0][1]=Fn.data[1][0]=0;//单位阵初始化
         c.data[0][0]= c.data[0][1]=c.data[1][0]=1;c.data[1][1]=0;//常数阵初始化
        Mat_qpow(Fn,n);
        cout<<Fn.data[0][1]<<endl;
    }
    return 0;
}
```
