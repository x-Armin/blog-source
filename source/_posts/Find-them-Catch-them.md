---
title: Find them,Catch them(种类并查集)
mathjax: true
copyright: true
date: 2018-03-21 14:28:42
tags: 并查集
categories: 题解
---
# 描述
传送门：[poj-1703](http://poj.org/problem?id=1703)

>&emsp;The police office in Tadu City decides to say ends to the chaos, as launch actions to root up the TWO gangs in the city, Gang Dragon and Gang Snake. However, the police first needs to identify which gang a criminal belongs to. The present question is, given two criminals; do they belong to a same clan? You must give your judgment based on incomplete information. (Since the gangsters are always acting secretly.) 

<!--more-->
>&emsp;Assume N (N <= 10^5) criminals are currently in Tadu City, numbered from 1 to N. And of course, at least one of them belongs to Gang Dragon, and the same for Gang Snake. You will be given M (M <= 10^5) messages in sequence, which are in the following two kinds: 

>1. D [a] [b] 
where [a] and [b] are the numbers of two criminals, and they belong to different gangs. 
<br/>
>2. A [a] [b] 
where [a] and [b] are the numbers of two criminals. This requires you to decide whether a and b belong to a same gang. 

## Input
>&emsp;The first line of the input contains a single integer T (1 <= T <= 20), the number of test cases. Then T cases follow. Each test case begins with a line with two integers N and M, followed by M lines each containing one message as described above.

## Output
>&emsp;For each message "A [a] [b]" in each case, your program should give the judgment based on the information got before. The answers might be one of "In the same gang.", "In different gangs." and "Not sure yet."

## Examples
* intput
```
1
5 5
A 1 2
D 1 2
A 1 2
D 2 4
A 1 4
```
* output
```
Not sure yet.
In different gangs.
In the same gang.
```

# 思路
>* 这是一道裸的种类并查集，种类并查集就是比普通并查集多维护一个种类数组group[]。
>* 这道题group[i]=1表示i和Fa[i]不是一个帮派。如何来维护group数组呢，我们需要修改一下Union函数和Find函数。
>* Union函数稍有变化,加了一句：
`group[fa]=(group[b]+1-group[a])%2;`画个图出来就很容易理解
<center>![图](http://wx3.sinaimg.cn/mw690/005ZgyPegy1fpkik2w5rvj30bw05xwef.jpg)

>* Find函数的`group[x]=(group[x]+group[tem])%2;`也是同理，画个图就好理解了。

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
const int M =1e5+5;

int Fa[M],group[M];

void init(int n)
{
    CRL(group);
    for(int i=0; i<=n; i++)
        Fa[i]=i;
}

int Find(int x)
{
    if(Fa[x]==x)
        return x;
    int tem=Fa[x];
    Fa[x]=Find(tem);
    group[x]=(group[x]+group[tem])%2;     //更新group数组，因为可能之前已经进行了一次合并，
    return Fa[x];                        // group[x]保存的就可能不是跟现在根节点的关系，而是跟tem的关系。
}

void Union(int a,int b)
{
    int fa=Find(a);
    int fb=Find(b);
    if(fa!=fb)
    {
        Fa[fa]=fb;
        group[fa]=(group[b]+1-group[a])%2;		//更新group数组
    }
}

int main()
{
    int Case,n,m,a,b;
    char x;
    cin>>Case;
    while(Case--)
    {
        cin>>n>>m;
        init(n);
        while(m--)
        {
            getchar();
            scanf("%c %d %d",&x,&a,&b);		//cin会超时
            if(x=='D')
                Union(a,b);
            else
            {
                if(Find(a)!=Find(b))
                    printf("Not sure yet.\n");
                else if(group[a]==group[b])
                    printf("In the same gang.\n");
                else
                    printf("In different gangs.\n");
            }
        }
    }
    return 0;
}

```

#类似题
>[poj-2492](http://poj.org/problem?id=2492)
>[poj-1182](http://poj.org/problem?id=1182)
