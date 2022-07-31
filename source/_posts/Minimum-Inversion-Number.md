---
title: Minimum Inversion Number(树状数组+离散化处理)
mathjax: true
copyright: true
date: 2018-04-24 00:32:43
tags: 树状数组
categories: 题解
---
# 描述
传送门：[hdu-1394](http://acm.hdu.edu.cn/showproblem.php?pid=1394)

>&emsp;The inversion number of a given number sequence $a_1, a_2, ..., a_n$ is the number of pairs $(a_i, a_j)$ that satisfy i < j and $a_i > a_j$.

> For a given sequence of numbers a1, a2, ..., an, if we move the first m >= 0 numbers to the end of the seqence, we will obtain another sequence. There are totally n such sequences as the following:

<!--more-->
> $a_1, a_2, ..., a_(n-1), a_n$ (where m = 0 - the initial seqence)
$a_2, a_3, ..., a_n, a_1$ (where m = 1)
$a_3, a_4, ..., a_n, a_1, a_2$ (where m = 2)
...
$a_n, a_1, a_2, ..., a_(n-1) (where m = n-1)

> You are asked to write a program to find the minimum inversion number out of the above sequences.

## Input
> The input consists of a number of test cases. Each case consists of two lines: the first line contains a positive integer n (n <= 5000); the next line contains a permutation of the n integers from 0 to n-1.

## Output
> For each case, output the minimum inversion number on a single line.

## Examples
* intput
```
10
1 3 6 9 0 8 5 7 4 2
```
* output
```
16
```

# 大致题意
> 给你一个长$n$的数列，每次把第一个数放到末尾，求这$n$种排列方式中的最小逆序数。

# 思路
>* 求出原始数列的逆序数。
>* O(n)遍历，每次求出新的数列的逆序数，每次变化得到的新的逆序数为$tem+(n-2*b[i]+1)$，b[i]为数列第i个值。
>* 求逆序数的方法有：暴力，归并排序，树状数组+离散化,这次我用的是树状数组。详见[求数列的逆序数](http://x-armin.com/%E6%B1%82%E6%95%B0%E5%88%97%E7%9A%84%E9%80%86%E5%BA%8F%E6%95%B0/)

# 代码
```c++
#include<bits/stdc++.h>
using namespace std;
#define CRL(a) memset(a,0,sizeof(a))
#define lowbit(x) (x&(-x))
#define INF 0xffffffff
typedef long long ll;
const int N=5e5+5;
int tr[N],b[N],n;
typedef pair <int,int> pp;
pp a[N];

void Update(int x){while(x<=n){tr[x]++;x+=lowbit(x);}}
int Query(int x){int sum=0;while(x>0){sum+=tr[x];x-=lowbit(x);}return sum;}

int main()
{
    while(cin>>n)
    {
        CRL(tr);
        for(int i=1;i<=n;i++) //离散化开始
        {
            cin>>a[i].first;
            a[i].second=i;
        }
        sort(a+1,a+n+1);
        for(int i=1;i<=n;i++) b[a[i].second]=i; //离散化结束
        
        int ans=0;
        for(int i=1;i<=n;i++)
        {
            Update(b[i]);
            ans+=(i-Query(b[i]));
        }
        
        int tem=ans;
        for(int i=1;i<n;i++)
        {
            tem=tem+(n-2*b[i]+1);
            ans=min(ans,tem);
        }
        cout<<ans<<endl;
    }
    return 0;
 } 
```
