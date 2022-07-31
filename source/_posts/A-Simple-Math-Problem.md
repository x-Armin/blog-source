---
title: A Simple Math Problem(矩阵快速幂)
mathjax: true
copyright: true
date: 2018-08-23 02:07:19
tags: [数论,矩阵快速幂]
categories: 题解
---
# 描述
传送门：[hdu-1757](http://acm.hdu.edu.cn/showproblem.php?pid=1757)

>&emsp;Lele now is thinking about a simple function $f(x)$.
>$$
>F(i)=
> \begin{cases}
>   f(x) = x &x<10\\
>   f(x) = a_0 * f(x-1) + a_1 * f(x-2) + a_2 * f(x-3) + …… + a_9 * f(x-10) &x>=10\\
>   \end{cases}
>$$
And $a_i(0<=i<=9)$ can only be 0 or 1 .

>Now, I will give $a_0 ~ a_9$ and two positive integers $k$ and $m$ ,and could you help Lele to caculate $f(k)%m$.

<!--more-->
## Input
> The problem contains mutiple test cases.Please process to the end of file.
In each case, there will be two lines.
In the first line , there are two positive integers $k$ and $m$. $( k<2*10^9 , m < 10^5 )$
In the second line , there are ten integers represent $a_0 ~ a_9$.

## Output
> For each case, output $f(k) % m$ in one line.

## Examples
* intput
```c++
10 9999
1 1 1 1 1 1 1 1 1 1
20 500
1 0 1 0 1 0 1 0 1 0
```
* output
```c++
45
104
```

# 思路
>* O(n)肯定会T，可以考虑[矩阵快速幂](http://x-armin.com/%E6%95%B0%E8%AE%BA%E7%AC%94%E8%AE%B0%E6%9C%AC/#%E7%9F%A9%E9%98%B5%E5%BF%AB%E9%80%9F%E5%B9%82)求解。
>* 由于题目给了递推式，所以很显然我们需要求一个$X$矩阵，使得这个矩阵满足
$$
\begin{equation*}
X^{n - 9}
\begin{bmatrix}
9\\
8\\
7\\
6\\
5\\
4\\
3\\
2\\
1\\
0
\end{bmatrix}=
X
\begin{bmatrix}
F(n-1))\\
F(n-2)\\
F(n-3)\\
F(n-4)\\
F(n-5)\\
F(n-6)\\
F(n-7)\\
F(n-8)\\
F(n-9)\\
F(n-10)
\end{bmatrix}=
\begin{bmatrix}
F(n)\\
F(n-1))\\
F(n-2)\\
F(n-3)\\
F(n-4)\\
F(n-5)\\
F(n-6)\\
F(n-7)\\
F(n-8)\\
F(n-9)
\end{bmatrix}
\end{equation*}
$$，易得$$X=\begin{bmatrix}
a_0&a_1&a_2&a_3&a_4&a_5&a_6&a_7&a_8&a_9\\
1 & 0&0&0&0&0&0&0&0&0\\
0 & 1&0&0&0&0&0&0&0&0\\
0 & 0&1&0&0&0&0&0&0&0\\
0 & 0&0&1&0&0&0&0&0&0\\
0 & 0&0&0&1&0&0&0&0&0\\
0 & 0&0&0&0&1&0&0&0&0\\
0 & 0&0&0&0&0&1&0&0&0\\
0 & 0&0&0&0&0&0&1&0&0\\
0 & 0&0&0&0&0&0&0&1&0\\
\end{bmatrix}
$$。
将$F_{n}$按递推式展开，取对应系数即可，
>* 递推式是从10开始的，所以对与第$n$项，只需求$X^{n-9}$，所得矩阵再左乘一个$\begin{bmatrix}F_9 & F_8 & F_7 & F_6 & F_5 & F_4& F_3& F_2& F_1& F_0\end{bmatrix}^{-1}$，所得矩阵的第一个元素即为$F_i$

# 代码
```c++
#include <bits/stdc++.h>
using namespace std;
#define rep(i,a,n) for(int i=a;i<n;i++)
#define repd(i,a,n) for(int i=n-1;i>=a;i--)
#define CRL(a,x) memset(a,x,sizeof(a))
typedef long long ll;
ll mod= 1e9+7;

struct Mat {        //矩阵
    ll data[15][15];
    Mat() {CRL(data,0);}
    Mat operator* (Mat &a) const {  //重载 * 运算符
        Mat c;
        rep(i,0,10)
            rep(j,0,10)
                rep(k,0,10)
                    c.data[i][j]=(c.data[i][j]+data[i][k]*a.data[k][j]%mod)%mod;
        return c;
    }
}a;

Mat Mat_qpow(Mat a,int n) {     //矩阵快速幂
    Mat tem;
    rep(i,0,10) tem.data[i][i]=1;
    while(n) {
        if(n&1LL)
            tem=tem*a;
        a=a*a;
        n>>=1LL;
    }
    return tem;
}

int main() {
    std::ios::sync_with_stdio(false);
    ll M[15][15] = {0},num[10],n,ans=0;
    while(cin>>n>>mod) {
        rep(i,0,10) cin>>num[i];

        if(n<10) {      //特判
            cout<<n<<endl;
            continue;
        }

        rep(i,0,10) M[i+1][i]=1,M[0][i]=num[i];     //初始化常数矩阵
        memcpy(a.data,M,sizeof(M));
        ans=0;

        a=Mat_qpow(a,n-9);          //只求n-9次方
        rep(i,0,10) ans=(ans+a.data[0][i]*(9-i)%mod)%mod;       //处理结果
        cout<<ans<<endl;
    }
    return 0;
}
```
