---
title: How Many Answers Are Wrong(带权并查集)
mathjax: true
copyright: true
date: 2018-03-21 18:34:33
tags: 并查集
categories: 题解
---
# 描述
传送门：[hdu-3038](http://acm.hdu.edu.cn/showproblem.php?pid=3038)

>TT and FF are ... friends. Uh... very very good friends -________-b

>FF is a bad boy, he is always wooing TT to play the following game with him. This is a very humdrum game. To begin with, TT should write down a sequence of integers-\_-!!(bored).

>Then, FF can choose a continuous subsequence from it(for example the subsequence from the third to the fifth integer inclusively). After that, FF will ask TT what the sum of the subsequence he chose is. The next, TT will answer FF's question. Then, FF can redo this process. In the end, FF must work out the entire sequence of integers.

<!--more-->
>Boring~~Boring~~a very very boring game!!! TT doesn't want to play with FF at all. To punish FF, she often tells FF the wrong answers on purpose.

>The bad boy is not a fool man. FF detects some answers are incompatible. Of course, these contradictions make it difficult to calculate the sequence.

>However, TT is a nice and lovely girl. She doesn't have the heart to be hard on FF. To save time, she guarantees that the answers are all right if there is no logical mistakes indeed.

>What's more, if FF finds an answer to be wrong, he will ignore it when judging next answers.

>But there will be so many questions that poor FF can't make sure whether the current answer is right or wrong in a moment. So he decides to write a program to help him with this matter. The program will receive a series of questions from FF together with the answers FF has received from TT. The aim of this program is to find how many answers are wrong. Only by ignoring the wrong answers can FF work out the entire sequence of integers. Poor FF has no time to do this job. And now he is asking for your help~(Why asking trouble for himself~~Bad boy)

## Input
>Line 1: Two integers, N and M (1 <= N <= 200000, 1 <= M <= 40000). Means TT wrote N integers and FF asked her M questions.

>Line 2..M+1: Line i+1 contains three integer: Ai, Bi and Si. Means TT answered FF that the sum from Ai to Bi is Si. It's guaranteed that 0 < Ai <= Bi <= N.

>You can assume that any sum of subsequence is fit in 32-bit integer.

## Output
>A single line with a integer denotes how many answers are wrong.

## Examples
* intput
```
10 5
1 10 100
7 10 28
1 3 32
4 6 41
6 6 1
```
* output
```
1
```

# 题意
> 有一个数列，给你几条语句，每条都是a,b,s形式，代表这个数列从a到b的和为s，求这些语句有几条是跟**它之前**的语句矛盾的。

# 思路
>* 这是一道裸的带权并查集，种类并查集就是比普通并查集多维护一个权值数组valum[]。~~其实和种类并查集很像，种类并查集就是权值都为1的带权并查集再膜一下种类数。~~
>* 这道题valum[i]表示i到fa[i]的总和。如何来维护valum数组呢，我们需要修改一下Union函数和Find函数。
>* Union函数稍有变化,加了一句：
`valum[fa]=s+valum[b]-valum[a];`画个图出来就很容易理解
<center>![图](http://wx2.sinaimg.cn/mw690/005ZgyPegy1fpkv45fw23j30cb06pglj.jpg)

>* Find函数的`valum[x]+=valum[tem];`也是同理，画个图就好理解了。

# 代码
```c++
//#include<bits/stdc++.h>
#include<iostream>
#include<algorithm>
#include<string.h>
#include<string>
#include<stdio.h>
#include<stdlib.h>
#include<math.h>
using namespace std;
#define CRL(a) memset(a,0,sizeof(a))
#define MAX 0xfffffff
typedef unsigned long long LL;
typedef  long long ll;
const double Pi = acos(-1);
const double e = exp(1.0);
const int mod =1e9+7;

int valum[200005],Fa[200005],n,time,ans=0;

void init(int n)
{
    for(int i=0; i<=n; i++)
        Fa[i]=i;
    CRL(valum);
    ans=0;
}

int Find(int x)
{
    if(Fa[x]!=x)
    {
        int tem=Fa[x];
        Fa[x]=Find(Fa[x]);
        valum[x]+=valum[tem];
    }
    return Fa[x];
}

void Union(int a,int b,int s)
{
    int fa=Find(a);
    int fb=Find(b);
    if(fa!=fb)
    {
        Fa[fa]=fb;
        valum[fa]=s+valum[b]-valum[a];
    }
    else if(valum[a]!=s+valum[b])
        ans++;
}

int main()
{
    ios::sync_with_stdio(false);   //加速cin,cout
    int a,b,s;
    while(cin>>n>>time)
    {
        init(n);
        while(time--)
        {
            cin>>a>>b>>s;
            Union(--a,b,s);   //因为元素是离散的，要--a,这样valum[b]-valum[a]；才是我们想要的区间和
        }
        cout<<ans<<endl;
    }
    return 0;
}
```

# 类似题
> [poj-1988](http://poj.org/problem?id=1988)