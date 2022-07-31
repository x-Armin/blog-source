---
title: Code(排列组合)
mathjax: true
copyright: true
date: 2018-07-30 02:18:58
tags: [排列组合,数学]
categories: 题解
---
# 描述
传送门：[poj-1850](http://poj.org/problem?id=1850)

>&emsp;Transmitting and memorizing information is a task that requires different coding systems for the best use of the available space. A well known system is that one where a number is associated to a character sequence. It is considered that the words are made only of small characters of the English alphabet a,b,c, ..., z (26 characters). From all these words we consider only those whose letters are in lexigraphical order (each character is smaller than the next character). 

<!--more-->

>The coding system works like this: 
>* The words are arranged in the increasing order of their length. 
>* The words with the same length are arranged in lexicographical order (the order from the dictionary). 
>* We codify these words by their numbering, starting with a, as follows:  
> a - 1  
> b - 2  
...  
z - 26  
ab - 27  
...  
az - 51  
bc - 52  
...  
vwxyz - 83681  
...  
>
> Specify for a given word if it can be codified according to this coding system. For the affirmative case specify its code. 

## Input
> The only line contains a word. There are some constraints: 
>* The word is maximum 10 letters length 
>* The English alphabet has 26 characters. 

## Output
> The output will contain the code of the given word, or 0 if the word can not be codified.

## Examples
* intput
```
bf
```
* output
```
55
```

# 思路
>* 对于长度为$N$的字符串应该先求长度为$N-1$的字符串有多少种。很明显可以看出长度为i的字符串就是从26个字母中选择i个字符组合（因为有大小限制，所以不是排列）。所以长度为$N-1$的字符串有：$
sum_{i=1}^{Len-1}C_{26}^{i}$种。
>* 对于剩下的，就从a[0]开始，每次计算这个字母开头的单词个数，遍历完就行了。

# 代码
```c++
#include<iostream>
#include<string.h>
using namespace std;
#define rep(i,a,n) for(int i=a;i<n;i++)
#define repd(i,a,n) for(int i=n-1;i>=a;i--)

int C(int n,int r){return r? C(n,r-1)*(n-r+1)/r:1;}

int main()
{
    std::ios::sync_with_stdio(false);
    int sum=0;char a[15];
    while(cin>>a){
        int Len=strlen(a);sum=0;

        rep(i,1,Len)
            if(a[i]<=a[i-1]) sum=-1;

        if(sum==-1) {cout<<0<<endl;continue;}

        rep(i,1,Len) sum+=C(26,i);
        rep(i,0,Len){
            char tem= i? a[i-1]+1:'a';
            while(tem<a[i])
                sum+=C('z'-tem++,Len-i-1);
        }
        cout<<sum+1<<endl;
    }
    return 0;
}

```
