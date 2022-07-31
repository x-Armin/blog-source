---
title: Max Area(贪心)
mathjax: true
copyright: true
date: 2018-08-03 17:47:58
tags: 贪心
categories: 题解
---
# 描述
传送门：[swustoj-24](http://acm.swust.edu.cn/#/Problems/24/-1?_k=dmubh4)

>&emsp;又是这道题，请不要惊讶，也许你已经见过了，那就请你再来做一遍吧。这可是wolf最骄傲的题目哦。在笛卡尔坐标系正半轴（x>=0,y>=0）上有n个点，给出了这些点的横坐标和纵坐标，但麻烦的是这些点的坐标没有配对好，你的任务就是将这n个点的横坐标和纵坐标配对好，使得这n个点与x轴围成的面积最大。

<!--more-->
## Input
> 在数据的第一行有一个正整数m,表示有m组测试实例。接下来有$m$行，每行表示一组测试实例。每行的第一个数$n$，表示给出了$n$个点，接着给出了$n$个$x$坐标和$n$个$y$坐标。（给出的$x$轴的数据不会重复，$y$轴数据也不会重复）$(m<5000,1<n<50)$

## Output
> 输出所计算的最大面积，结果保留两位小数，每组数据占一行。

## Examples
* intput
```c++
2
4 0 1 3 5 1 2 3 4
6 14 0 5 4 6 8 1 5 6 2 4 3
```
* output
```c++
15.00
59.00
```

# 思路
>* 暴力枚举$O(n!)$肯定会T，数据范围就是赤果果的暗示要贪心。
>* 求面积就是把多边形垂直x轴切成n-1个梯形然后分开算没什么好说的，常规sort一下也没什么好说的。
>* 然后就会发现每一个$y_i$都会和$x_i-x_{i-1},\ x_{i+1}-x_{i}$这两个高乘一下，合并一下答案就是**$$\sum_{i=0}^{n-1} (x_{i+1}-x_{i-1}) \times y_i$$**，当然，还要处理一下两边的越界。
>* 坑点：数据都是 **$double$**

# 代码
```c++
#include<bits/stdc++.h>
using namespace std;
#define rep(i,a,n) for(int i=a;i<n;i++)
#define repd(i,a,n) for(int i=n-1;i>=a;i--)
const int N=5005;

int main() {
    double x[N],y[N],h[N],ans;int n,T;
    cin>>T;
    while(T--){
        cin>>n;ans=0;
        rep(i,0,n)cin>>x[i];
        rep(i,0,n)cin>>y[i];
        sort(x,x+n);
        h[0]=x[1]-x[0];h[n-1]=x[n-1]-x[n-2];
        rep(i,1,n-1) h[i]=x[i+1]-x[i-1];
        sort(h,h+n);
        sort(y,y+n);
        rep(i,0,n) ans+=h[i]*y[i];
        printf("%.2lf\n",ans/2);
    }
    return 0;
}
```
