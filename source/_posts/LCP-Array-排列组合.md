---
title: LCP Array(排列组合)
date: 2018-02-20 23:20:20
mathjax: true
copyright: true
tags: [排列组合,数学]
categories: 题解
---
# 描述
传送门：[hdu-5635](http://acm.hdu.edu.cn/showproblem.php?pid=5635)

>Peter has a string $s=s_1s_2...s_n$, let $suff_i=s_is_{i+1}...s_n$ be the suffix start with $i^{th}$ character of $s$. Peter knows the **lcp** (longest common prefix) of each two adjacent suffixes which denotes as $a_i=lcp(suff_i,suff_{i+1})(1≤i<n)$.

>Given the lcp array, Peter wants to know how many strings containing lowercase English letters only will satisfy the lcp array. The answer may be too large, just print it modulo $10^9+7$.


<!--more-->
## Input
>There are multiple test cases. The first line of input contains an integer T indicating the number of test cases. For each test case:

>The first line contains an integer n ($2≤n≤10^5$) -- the length of the string. The second line contains n−1 integers: $a_1,a_2,...,a_{n−1} (0≤a_i≤n)$.

>The sum of values of n in all test cases doesn't exceed $10^6$.


## Output
>For each test case output one integer denoting the answer. The answer must be printed modulo $10^9+7$.

## Examples
* intput
```
3
3
0 0
4
3 2 1
3
1 2
```
* output
```
16250
26
0
```

# 思路
>* **有解的条件**：以样例2为例:$lcp_1=3 \Rightarrow s_1=s_2,s_2=s_3,s_3=s_4$,要有解显然$lcp_i=lcp_{i-1}-1$一定成立，而且$lcp_{n-1}$不可能大于1,也就是说如果$lcp_i$=n,那么$lcp_i$到$lcp_{i+n}$的值一定是n到1递减。
>* __计算有解时的答案__：显然当$lcp_i$=n(n>0)时，$s_i$到$s_{i+n}$都为同一个字母，如果$lcp_i$=0,则$s_i$和$s_{i+1}$为不同字母，所以当前一种字母确定时，后一种字母就有25种可能。
>* 所以如果有解，lcp数组中0的个数为n,答案就是$26 \times 25^n$。~~我也是在草稿本上先找到规律AC了才推出原理的~~

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
#define M 100000
typedef unsigned long long LL;
typedef  long long ll;
const int mod =1e9+7;

int main()
{
    int n,len,lcp[M],Flag=0;
    ll ans;
    cin>>n;
    while(n--)
    {
        Flag=0;
        ans=26;
        cin>>len;
        for(int i=1; i<len; i++)
        {
            scanf("%d",&lcp[i]);
            if(lcp[i]+1!=lcp[i-1]&&lcp[i-1]>0)  //如果不是前比后多一且前一个不为0
                Flag=1;
            else if(lcp[i]==0)
                ans=(ans*25)%mod;
        }
        if(Flag||lcp[len-1]>1)  //lcp数组末尾一定是0或1
            cout<<0<<endl;
        else
            cout<<ans<<endl;
    }
    return 0;
}
```