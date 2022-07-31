---
title: Who Gets the Most Candies?(线段树+约瑟夫环)
mathjax: true
copyright: true
date: 2018-04-25 21:25:13
tags: 线段树
categories: 题解
---
# 描述
传送门：[poj-2886](http://poj.org/problem?id=2886)

>&emsp;N children are sitting in a circle to play a game.

> The children are numbered from 1 to N in clockwise order. Each of them has a card with a non-zero integer on it in his/her hand. The game starts from the K-th child, who tells all the others the integer on his card and jumps out of the circle. The integer on his card tells the next child to jump out. Let A denote the integer. If A is positive, the next child will be the A-th child to the left. If A is negative, the next child will be the (−A)-th child to the right.

> The game lasts until all children have jumped out of the circle. During the game, the p-th child jumping out will get F(p) candies where F(p) is the number of positive integers that perfectly divide p. Who gets the most candies?

<!--more-->
## Input
> There are several test cases in the input. Each test case starts with two integers N $(0 < N \leq 500,000)$ and K $(1 \leq  K \leq  N) on the first line. The next N lines contains the names of the children (consisting of at most 10 letters) and the integers (non-zero with magnitudes within $10^8$) on their cards in increasing order of the children’s numbers, a name and an integer separated by a single space in a line with no leading or trailing spaces.

## Output
> Output one line for each test case containing the name of the luckiest child and the number of candies he/she gets. If ties occur, always choose the child who jumps out of the circle first.

## Examples
* intput
```
4 2
Tom 2
Jack 4
Mary -1
Sam 1
```
* output
```
Sam 3
```

# 题目大意
>* 有N个人，每个人有一个数$a_i$。从第K个人开始出列，如果他的数$a_i$>0，则他顺时针方向第$(a_i)$个人出列，循环，直到最后一个人出列。
>* $x$表示在第$x$轮出列,$f(x)$表示$x$的因数个数。求最大的$f(x)$。

# 思路
>* 线段树维护区间值和，表示这个区间还剩多少人，每次更新就在对应区间-1。
>* 设当前这轮$k$出列，用$k=(k+a_i$)%当前剩余人数，求得下一个$k$的值。

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
#define CRL(a) mamset(a,0,sizeof(a))
typedef long long ll;
const int N=5e5+5;
typedef pair<char[12],int > pp;
int tr[N<<2],book[N];
pp people[N];

int Init()		//打表book[i]表示i的因数个数 
{
    for(int i=1;i<=500000;i++)
        for(int j=i;j<=500000;j+=i)
            book[j]++;
}

void Built(int root,int l,int r)
{	
    if(l==r) { tr[root]=1; return;}
    else{
        int mid=l+r>>1;
        Built(root<<1,l,mid);
        Built(root<<1|1,mid+1,r);
        tr[root]=tr[root<<1]+tr[root<<1|1];
    }
}

int Update(int root,int l,int r,int x) 
{
    if(l==r) {
        tr[root]--;
        return l;
    }
    else{
        tr[root]--;
        int mid=l+r>>1;
        if(x<=tr[root<<1])	return Update(root<<1,l,mid,x);		//判断出列的在左边还是右边 
        else   return Update(root<<1|1,mid+1,r,x-tr[root<<1]);
    }
}

int main()
{
    ios::sync_with_stdio(false);
    Init();
    int n,k,ans=0,tem;
    while(cin>>n>>k)
    {
        ans=0;tem=k;
        Built(1,1,n);
        for(int i=1;i<=n;i++)
            cin>>people[i].first>>people[i].second;
        
        for(int i=1;i<=n;i++)
            if(ans<book[i]) ans=book[i];	//找到最大的f(x) 
        
        for(int i=1;i<=n;i++)		//依次出列 
        {
            tem=Update(1,1,n,k);	//第k个人出列 
            if(book[i]==ans)
            {
                cout<<people[tem].first<<" "<<ans<<endl;
                break;
            }
            
            if(people[tem].second>0)  //判断下一个出列的人的位置 
                k=(k-1+people[tem].second-1)%(n-i)+1; 
            else    
                k=((k-1+people[tem].second)%(n-i)+(n-i))%(n-i)+1; 
        }
    }
    return 0;
}
```
