---
title: How Many Tables(并查集入门)
mathjax: true
copyright: true
date: 2018-03-10 09:29:15
tags: [并查集,图论]
categories: 题解
---
# 描述
传送门：[HDU-1213 ](http://acm.hdu.edu.cn/showproblem.php?pid=1213)

>&emsp;Today is Ignatius' birthday. He invites a lot of friends. Now it's dinner time. Ignatius wants to know how many tables he needs at least. You have to notice that not all the friends know each other, and all the friends do not want to stay with strangers.
&emsp;One important rule for this problem is that if I tell you A knows B, and B knows C, that means A, B, C know each other, so they can stay in one table.
&emsp;For example: If I tell you A knows B, B knows C, and D knows E, so A, B, C can stay in one table, and D, E have to stay in the other one. So Ignatius needs 2 tables at least.

<!--more-->
## Input
>&emsp;The input starts with an integer T(1<=T<=25) which indicate the number of test cases. Then T test cases follow. Each test case starts with two integers N and M(1<=N,M<=1000). N indicates the number of friends, the friends are marked from 1 to N. Then M lines follow. Each line consists of two integers A and B(A!=B), that means friend A and friend B know each other. There will be a blank line between two cases.

## Output
>For each test case, just output how many tables Ignatius needs at least. Do NOT print any blanks.

## Examples
* intput
```
2
5 3
1 2
2 3
4 5

5 1
2 5
```
* output
```
2
4
```

# 思路
>* 并查集入门题。初始化每人单独为一个集合，每次读入A,B就合并A,B所在的集合，每个集合都有一个**代表元素**，集合中的元素都指向代表元素并且只有代表元素指向自己，最后遍历一遍求出答案。
>* 同样的方法也可以求无向图里的连通分支数。

# 代码
```c++
//#include<bits/stdc++.h>
using namespace std;
#define CRL(a) memset(a,0,sizeof(a))
#define MAX 0xfffffff
typedef unsigned long long LL;
typedef  long long ll;
const double Pi = acos(-1);
const double e = 2.718281828459;
const int mod =1e9+7;

int F[1005];

int f(int x) {
    return F[x]==x? x:F[x]=f(F[x]);
}

void Union(int a,int b){    //合并
    a=f(a);
    b=f(b);
    if(a!=b) F[a]=b;
}

int main() {
    int test,n,way,a,b,ans=0;
    cin>>test;
    while(test--) {
        ans=0;
        cin>>n>>way;
        for(int i=0; i<=n; i++) F[i]=i;
        for(int i=0; i<way; i++) {
            cin>>a>>b;
            Union(a,b);
        }

        for(int i=1; i<=n; i++)		//查找有几个代表元素
            if(F[i]==i)
                ans++;

        cout<<ans<<endl;
    }
    return 0;
}


```
