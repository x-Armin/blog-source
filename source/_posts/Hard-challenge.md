---
title: Hard challenge(模拟)
mathjax: true
copyright: true
date: 2018-04-29 00:01:25
tags: 模拟
categories: 题解
---
# 描述
传送门：[hdu-6127](http://acm.hdu.edu.cn/showproblem.php?pid=6127)

>&emsp;There are n points on the plane, and the ith points has a value $val_i$, and its coordinate is $(x_i,y_i)$. It is guaranteed that no two points have the same coordinate, and no two points makes the line which passes them also passes the origin point. For every two points, there is a segment connecting them, and the segment has a value which equals the product of the values of the two points. Now HazelFan want to draw a line throgh the origin point but not through any given points, and he define the score is the sum of the values of all segments that the line crosses. Please tell him the maximum score.

<!--more-->
## Input
> The first line contains a positive integer $T(1≤T≤5)$, denoting the number of test cases.
For each test case:
The first line contains a positive integer $n(1≤n≤5×10^4)$.
The next n lines, the ith line contains three integers $x_i,yi,val_i(|x_i|,|y_i|≤10^9,1≤val_i≤10^4)$.

## Output
> For each test case:
A single line contains a nonnegative integer, denoting the answer.

## Examples
* intput
```
2
2
1 1 1
1 -1 1
3
1 1 1
1 -1 10
-1 0 100
```
* output
```
1
1100
```

# 大致题意
> 有$n$个点，每个点都有一个值，两个点确定的线段的值为两个点的乘积。求一条过原点的直线，使它穿过的所有线段的值之和最大，输出这个最大值。

# 思路
>* 第一次做这种题完全没有什么思路，想到的算法还没敲都被自己YY TLE了，最后还是靠大佬点拨了一下按斜率排序。（果然还是太菜了）
>*  易得**直线穿过的值之和就是两块区域中点的和的乘积**。
>* 对于每个点，我们求这个点和原点连线的斜率，然后对所有的点按斜率排序。
>* 先选一条直线，将所有的点分为两部分，然后慢慢旋转180°。由于我们对斜率排序，所以我们选择x=0为初始直线。
>* 我们将x=0这条直线逆时针旋转，可以想象一下它在旋转的时候，会依次碰到排序后的点，（由于题目说了不会存在两点连线过原点的情况，所以不会同时碰到两个点。~~一开始没看到，想了好久~~）。每碰到一个点，就将它的值从原集合减去加到另一个集合中。找到所有分法的最大值。
>* ans要用long long。

# 代码
```c++
#include<bits/stdc++.h>
using namespace std;
#define CRL(a) memset(a,0,sizeof(a))
#define INF 0xfffffff
typedef unsigned long long LL;
typedef  long long ll; 
const double Pi = acos(-1);
const double e = exp(1.0);
const int mod =1e9+7; 
const int N=5e4+5;

struct node{
    int x,y,valum;
    double Angle;
}point[N];

bool cmp(node x,node y){return x.Angle<y.Angle;}

int main()
{
    int Case,n;ll sum1,sum2,ans;
    cin>>Case;
    while(Case--)
    {
        sum1=0;sum2=0;
        cin>>n;
        for(int i=0;i<n;i++){
            scanf("%d%d%d",&point[i].x,&point[i].y,&point[i].valum);
            point[i].Angle=point[i].y*1.0/point[i].x;          //算斜率
        }
        
        sort(point,point+n,cmp);
        
        for(int i=0;i<n;i++)        //初始时直线是x=0,所以x>0是一个集合，x<=0是个集合。
            if(point[i].x>0) 
                sum1+=point[i].valum;
            else 
                sum2+=point[i].valum;
        ans=sum1*sum2;

        for(int i=0;i<n;i++){       //旋转
            if(point[i].x>0) 
                sum1-=point[i].valum,sum2+=point[i].valum;
            else 
                sum1+=point[i].valum,sum2-=point[i].valum;
            ans=max(ans,sum1*sum2);      //取最大
        }
            
        cout<<ans<<endl;
    }
    return 0;
}
```
