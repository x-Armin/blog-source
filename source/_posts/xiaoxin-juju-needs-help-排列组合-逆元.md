---
title: xiaoxin juju needs help(排列组合+逆元)
mathjax: true
copyright: true
date: 2018-02-21 23:15:31
tags: [排列组合,数论,逆元,数学]
categories: 题解
---
# 描述
传送门：[hdu-5651](http://acm.hdu.edu.cn/showproblem.php?pid=5651)

>As we all known, xiaoxin is a brilliant coder. He knew **palindromic** strings when he was only a six grade student at elementry school.

>This summer he was working at Tencent as an intern. One day his leader came to ask xiaoxin for help. His leader gave him a string and he wanted xiaoxin to generate palindromic strings for him. Once xiaoxin generates a different palindromic string, his leader will give him a watermelon candy. The problem is how many candies xiaoxin's leader needs to buy?

<!--more-->
## Input
>This problem has multi test cases. First line contains a single integer T(T≤20) which represents the number of test cases.
For each test case, there is a single line containing a string S(1≤length(S)≤1,000).

## Output
>For each test case, print an integer which is the number of watermelon candies xiaoxin's leader needs to buy after mod 1,000,000,007.

## Examples
* intput
```
3
aa
aabb
a
```
* output
```
1
2
1
```

# 思路
>* 其实这道题难点不是推出公式，而是求解公式(吐血.jpg)。
>* 设字符串为s,字符串长度为Len,字母i出现次数的二分之一为c[i]\(取整);
>* 如果有解，就要求出现次数是奇数的字母不超过1个(废话)，然后我们只需要考虑一边的所有情况，另一边跟它一样就能构成回文序列。只考虑一边时，共有Len/2个字符,根据高中学的排列组合可知，所有的排列情况是
>$$ \frac{A_{Len/2}^{Len/2}}{A_{c[1]}^{c[1]}A_{c[2]}^{c[2]}...{A_{c[i]}^{c[i]}}} \Longrightarrow \frac{Len/2!}{c[1]!c[2]!...c[i]!} $$
>* 关键就是如何求解这个公式了，由题目可知，Len/2最大是500，也就是要算500!，很明显要爆精度。因为答案对(1e9+7)取摸，这里引入一个转化：
>(a$ \times $b)%m $\rightarrow$ (a%m $ \times $ b%m )%m  显然正确,所以我们求阶乘的时候只要边乘边对(1e9+7)取模就不会爆精度。
>但是 (a $ \div $ b)%m $ \not= $ ((a%m) $ \div $ (b%m))%m,所以我们要用逆元变除法为乘法，就可以边乘边取模。
>* 这道题我是用的拓展欧几里德算法求逆元，不知道的可以看我的另一篇博客[数论笔记本](http://x-armin.com/数论笔记本/)


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
#define CRL(a) memset(a,0,sizeof(a))
#define MAX 0xfffffff
#define MAXN 6
typedef unsigned long long LL;
typedef  long long ll;
const int mod =1e9+7;

ll  fac(int n)	//求阶乘 
{
	if(n==1||n==0)
		return 1;
	else
	{
		ll ans=1;
		for(int i=2;i<=n;i++)
            ans=(ans*i)%mod;		//边乘边取模 
        return ans;
	}
}

int x,y;
int exgcd(int a,int b)		//拓展欧几里德算法，求出的x,即为a%b下a的逆元 
{
    if(b==0)
    {
        x=1;y=0;
        return a;
    }
    int r=exgcd(b,a%b);
    int c=x;
    x=y;
    y=c-a/b*y;
    return r;
}

int main()
{
    ll ans;
    int book[26],n,Flag=0,Len;
    char s[1005];
    cin>>n;
    while(n--)
    {
        CRL(book);
        Flag=0;
        ans=0;
        scanf("%s",s);
        Len=strlen(s);
        for(int i=0; i<Len; i++)
            book[s[i]-'a']++;
        for(int i=0; i<26; i++)
            if(book[i]&1)
                Flag++;
        if(Flag>1)		//如果出现次数为奇数的字母超过1,无解 
            ans=0;
        else
        {
            ans=fac(Len/2);
            for(int i=0; i<26; i++)
                if(book[i]>1)
                {
                    exgcd(fac(book[i]/2),mod);
                    x=x<0? x+mod:x;			//x小于0的话要转换成正数 
                    ans=ans*x%mod;
                }
        }
        cout<<ans<<endl;
    }
    return 0;
}
```
