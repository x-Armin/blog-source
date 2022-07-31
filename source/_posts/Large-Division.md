---
title: Large Division(数论)
mathjax: true
copyright: true
date: 2018-05-15 11:56:04
tags: 数论
categories: 题解
---
# 描述
传送门：[Light oj-1214](http://lightoj.com/volume_showproblem.php?problem=1214)

>&emsp;Given two integers, **a** and **b**, you should check whether a is divisible by **b** or not. We know that an integer **a** is divisible by an integer **b** if and only if there exists an integer **c** such that **a = b * c**.

<!--more-->
## Input
> Input starts with an integer **T (≤ 525)**, denoting the number of test cases.

Each case starts with a line containing two integers **$a (-10^200 ≤ a ≤ 10^200)$** and **b (|b| > 0, b fits into a 32 bit signed integer).** Numbers will not contain leading zeroes.

## Output
> For each case, print the case number first. Then print **'divisible'** if **a** is divisible by **b**. Otherwise print **'not divisible'**.

## Examples
* intput
```
6
101 101
0 67
-101 101
7678123668327637674887634 101
11010000000000000000 256
-202202202202000202202202 -101
```
* output
```
Case 1: divisible
Case 2: divisible
Case 3: divisible
Case 4: not divisible
Case 5: divisible
Case 6: divisible
```

# 思路
>* 题目就是问是否 a%b==0;
>* 两百位的数肯定不能直接算，高精度除法说不定能过。
>* 把a换一种方式表示，比如1234=((1\*10+2)\*10+3)*10+4。两边同时取模，由于(a+b)%c==a%c+b%c,所以可以分步取模。
>* 时间复杂度O(N)。

# 代码
```c++
#include <bits/stdc++.h>
using namespace std;
char a[205];
long long x,sum;

int main()
{
    int T;
    cin>>T;
    for(int Case=1;Case<=T;Case++)
    {
        sum=0;
        scanf("%s%lld",a,&x);
        int len=strlen(a);
        for(int i= a[0]=='-'? 1:0;i<len;i++)
            sum=(sum*10+(a[i]-'0'))%x;

        cout<<"Case "<<Case<<(sum? ": not divisible":": divisible")<<endl;
    }
    return 0;
}
```
