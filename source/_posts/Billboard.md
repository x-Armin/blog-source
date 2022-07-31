---
title: Billboard（线段树）
mathjax: true
copyright: true
date: 2018-04-22 12:01:42
tags: 线段树
categories: 题解
---
# 描述
传送门：[hdu-2795](http://acm.hdu.edu.cn/showproblem.php?pid=2795)

>&emsp;At the entrance to the university, there is a huge rectangular billboard of size h*w (h is its height and w is its width). The board is the place where all possible announcements are posted: nearest programming competitions, changes in the dining room menu, and other important information. 

<!--more-->
> On September 1, the billboard was empty. One by one, the announcements started being put on the billboard. 

> Each announcement is a stripe of paper of unit height. More specifically, the i-th announcement is a rectangle of size 1 * wi. 

> When someone puts a new announcement on the billboard, she would always choose the topmost possible position for the announcement. Among all possible topmost positions she would always choose the leftmost one. 

> If there is no valid location for a new announcement, it is not put on the billboard (that's why some programming contests have no participants from this university). 

> Given the sizes of the billboard and the announcements, your task is to find the numbers of rows in which the announcements are placed.

## Input
> There are multiple cases (no more than 40 cases). 

> The first line of the input file contains three integer numbers, h, w, and n (1 <= h,w <= $10^9$; 1 <= n <= 200,000) - the dimensions of the billboard and the number of announcements. 

> Each of the next n lines contains an integer number wi (1 <= wi <=$ 10^9 $) - the width of i-th announcement.

## Output
> For each announcement (in the order they are given in the input file) output one number - the number of the row in which this announcement is placed. Rows are numbered from 1 to h, starting with the top row. If an announcement can't be put on the billboard, output "-1" for this announcement.

## Examples
* intput
```
3 5 5
2
4
3
3
3
```
* output
```
1
2
1
3
-1
```

# 思路
>* 线段树，对每行建树，叶子节点为此行剩余的宽度，维护区间最大值。
>* 单点修改，单点查询。
>* 每次贴广告就更新一次，如果根节点左右都放不下就返回-1。 

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
const int N=2*1e6+5;

int tr[N<<2];

void Built(int root,int l,int r,int c)
{
    if(l==r) tr[root]=c;
    else
    {
        int mid=l+r>>1;
        Built(root<<1,l,mid,c);
        Built(root<<1|1,mid+1,r,c);

        tr[root]=max(tr[root<<1],tr[root<<1|1]);
    }
}

int Update(int root,int l,int r,int c)
{
    int tem;
    if(l==r)
    {
        if(tr[root]<c) return -1;
        tr[root]-=c;
        return l;
    }
    else
    {
        int mid=l+r>>1;
        if(tr[root<<1]>=c)  tem=Update(root<<1,l,mid,c);
        else if(tr[root<<1|1]>=c) tem=Update(root<<1|1,mid+1,r,c);
        else
            return -1;
        tr[root]=max(tr[root<<1],tr[root<<1|1]);
    }
    return tem;
}

int main()
{
    int h,w,n,x;
    ios::sync_with_stdio(false);
    while(cin>>h>>w>>n)
    {
        h= h>n? n:h;
        Built(1,1,h,w);
        while(n--)
        {
            cin>>x;
            cout<<Update(1,1,h,x)<<endl;
        }
    }
    return 0;
}
```
