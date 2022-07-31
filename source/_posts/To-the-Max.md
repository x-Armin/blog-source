---
title: To the Max（最大子矩阵）
mathjax: true
copyright: true
date: 2018-08-15 01:15:44
tags: dp
categories: 题解
---
# 描述
传送门：[hdu-1081](http://acm.hdu.edu.cn/showproblem.php?pid=1081)

>&emsp;Given a two-dimensional array of positive and negative integers, a sub-rectangle is any contiguous sub-array of size 1 x 1 or greater located within the whole array. The sum of a rectangle is the sum of all the elements in that rectangle. In this problem the sub-rectangle with the largest sum is referred to as the maximal sub-rectangle.

<!--more-->
> As an example, the maximal sub-rectangle of the array:

>&emsp;0 -2 -7 0
&emsp;9 &nbsp;2 -6 2
&nbsp;-4 &nbsp;1 -4 1
&nbsp;-1 &nbsp;8 0 -2

> is in the lower left corner:

>&emsp;9 2
&nbsp;-4 1
&nbsp;-1 8

>and has a sum of 15.

## Input
> &emsp;The input consists of an N x N array of integers. The input begins with a single positive integer N on a line by itself, indicating the size of the square two-dimensional array. This is followed by N 2 integers separated by whitespace (spaces and newlines). These are the N 2 integers of the array, presented in row-major order. That is, all numbers in the first row, left to right, then all numbers in the second row, left to right, etc. N may be as large as 100. The numbers in the array will be in the range [-127,127].

## Output
> Output the sum of the maximal sub-rectangle.

## Examples
* intput
```c++
4
0 -2 -7 0 
9  2 -6 2
-4 1 -4 1 
-1 8 0 -2
```
* output
```c++
15
```

# 思路
>* 求最大子矩阵，可借用最大子段和的思想来做，如下图，要求宽为$(i-j)$这么宽的最大区间和，就可以把i到j行按列加到一起，然后就转换成了求一维数组最大子段和，每一个元素就是红色区域。
>* 对于矩阵$a[n][n]$，构造一个dp数组，使$dp[i][j]$为$dp[0][0]$到$dp[i][j]$的和。那么下图矩阵a的红色区域就可以表示为: **$dp[i][k]-dp[i][k-1]-dp[j-1][k]+dp[j-1][k-1]$**，这样就实现了$O(1)$查找区间和，而dp数组只需$O(N^2)$就能求出。
>
>![](http://wx1.sinaimg.cn/mw690/005ZgyPegy1fufdmn4g3aj30m00gmdfz.jpg)

# 代码
```c++
#include<bits/stdc++.h>
using namespace std;
#define rep(i,a,n) for(int i=a;i<n;++i)
#define repd(i,a,n) for(int i=n-1;i>=a;--i)
#define CRL(a) memset(a,0,sizeof(a))
const int N=105;
int dp[N][N];

int AC(int n){

    rep(i,1,n+1)            //求dp数组
        rep(j,1,n+1)
            dp[i][j]+=dp[i-1][j]+dp[i][j-1]-dp[i-1][j-1];

    int Max=dp[1][1];
    rep(i,1,n+1)            //i,j,k即为上图的i,j,k
        rep(j,1,i+1){
            int sum=0;
            rep(k,1,n+1){
                int tem=dp[i][k]-dp[i][k-1]-dp[j-1][k]+dp[j-1][k-1];
                sum= sum>0? sum+tem:tem;
                if(sum>Max) Max=sum;
            }
        }
    return Max;
}

int main()
{
    std::ios::sync_with_stdio(false);
    int a[N][N],n;
    while(cin>>n){
        rep(i,1,n+1)
        rep(j,1,n+1)
            cin>>dp[i][j];

        cout<<AC(n)<<endl;
    }
    return 0;
}
```
